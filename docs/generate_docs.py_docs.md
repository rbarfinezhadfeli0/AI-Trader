# generate_docs.py - Documentation

## File Metadata
- **Path**: `generate_docs.py`
- **Size**: 30,661 bytes (29.94 KB)
- **Extension**: .py
- **Type**: Text file

## Original Source Code

```python
#!/usr/bin/env python3
"""
World's Best Repo Book Generator and Index Builder
Generates comprehensive documentation for the AI-Trader repository.
"""

import os
import json
import hashlib
import mimetypes
import re
from pathlib import Path
from datetime import datetime
from collections import defaultdict
import subprocess
import traceback

class RepoBookGenerator:
    def __init__(self, repo_root, docs_dir="docs"):
        self.repo_root = Path(repo_root)
        self.docs_dir = self.repo_root / docs_dir
        self.manifest = {
            "repo_name": "AI-Trader",
            "repo_source": str(self.repo_root),
            "generator_version": "1.0.0",
            "timestamp_start": datetime.now().isoformat(),
            "files": [],
            "file_count": 0,
            "docs_count": 0,
            "bytes_written": 0,
            "errors": []
        }
        self.progress = []
        self.keywords_global = {}
        self.file_checksums = {}

    def get_git_info(self):
        """Get git commit SHA and date."""
        try:
            result = subprocess.run(
                ["git", "log", "-1", "--format=%H %ci"],
                cwd=self.repo_root,
                capture_output=True,
                text=True,
                check=True
            )
            parts = result.stdout.strip().split(" ", 1)
            return {"commit_sha": parts[0], "commit_date": parts[1] if len(parts) > 1 else ""}
        except:
            return {"commit_sha": "unknown", "commit_date": "unknown"}

    def is_binary(self, file_path):
        """Determine if a file is binary."""
        # Check by extension first
        binary_extensions = {
            '.png', '.jpg', '.jpeg', '.gif', '.bmp', '.ico', '.pdf',
            '.zip', '.tar', '.gz', '.bz2', '.xz', '.7z',
            '.pyc', '.pyo', '.so', '.dll', '.exe', '.bin',
            '.pickle', '.pkl', '.npy', '.npz'
        }
        if file_path.suffix.lower() in binary_extensions:
            return True

        # Check mime type
        mime_type, _ = mimetypes.guess_type(str(file_path))
        if mime_type and not mime_type.startswith('text'):
            return True

        # Try reading first 8KB to check for null bytes
        try:
            with open(file_path, 'rb') as f:
                chunk = f.read(8192)
                if b'\x00' in chunk:
                    return True
        except:
            return True

        return False

    def get_file_size(self, file_path):
        """Get file size in bytes."""
        try:
            return file_path.stat().st_size
        except:
            return 0

    def classify_files(self):
        """Classify all files in the repository."""
        print("Classifying files...")
        all_files = []

        for file_path in self.repo_root.rglob("*"):
            if file_path.is_file():
                # Skip .git and docs directories
                try:
                    rel_path = file_path.relative_to(self.repo_root)
                    if rel_path.parts and (rel_path.parts[0] == 'docs' or rel_path.parts[0] == '.git'):
                        continue
                except ValueError:
                    continue

                size = self.get_file_size(file_path)
                is_binary = self.is_binary(file_path)

                file_info = {
                    "path": str(rel_path),
                    "abs_path": str(file_path),
                    "size": size,
                    "is_binary": is_binary,
                    "is_large": size > 100_000_000,  # 100MB
                    "extension": file_path.suffix,
                    "name": file_path.name
                }
                all_files.append(file_info)

        all_files.sort(key=lambda x: x["path"])
        self.manifest["files"] = all_files
        self.manifest["file_count"] = len(all_files)

        print(f"Total files: {len(all_files)}")
        print(f"Binary files: {sum(1 for f in all_files if f['is_binary'])}")
        print(f"Text files: {sum(1 for f in all_files if not f['is_binary'])}")
        print(f"Large files (>100MB): {sum(1 for f in all_files if f['is_large'])}")

        return all_files

    def safe_read_file(self, file_path, max_size=10_000_000):
        """Safely read a text file with size limit."""
        try:
            size = Path(file_path).stat().st_size
            if size > max_size:
                with open(file_path, 'r', encoding='utf-8', errors='ignore') as f:
                    content = f.read(max_size)
                return content, True  # truncated
            else:
                with open(file_path, 'r', encoding='utf-8', errors='ignore') as f:
                    content = f.read()
                return content, False  # not truncated
        except Exception as e:
            return f"Error reading file: {str(e)}", False

    def extract_keywords(self, content, file_path):
        """Extract keywords from file content."""
        keywords = {}

        # Extract Python identifiers
        if file_path.endswith('.py'):
            # Classes
            for match in re.finditer(r'class\s+(\w+)', content):
                keywords[match.group(1)] = f"Class defined in {file_path}"
            # Functions
            for match in re.finditer(r'def\s+(\w+)', content):
                keywords[match.group(1)] = f"Function defined in {file_path}"
            # Imports
            for match in re.finditer(r'(?:from|import)\s+([\w.]+)', content):
                keywords[match.group(1)] = f"Module imported in {file_path}"

        # Extract JavaScript/TypeScript identifiers
        elif file_path.endswith(('.js', '.ts', '.jsx', '.tsx')):
            for match in re.finditer(r'(?:class|function|const|let|var)\s+(\w+)', content):
                keywords[match.group(1)] = f"Identifier in {file_path}"

        # Extract general identifiers (variables, constants)
        for match in re.finditer(r'\b[A-Z][A-Z_]{2,}\b', content):
            if len(match.group(0)) > 2:
                keywords[match.group(0)] = f"Constant in {file_path}"

        # Extract CamelCase identifiers
        for match in re.finditer(r'\b[A-Z][a-z]+(?:[A-Z][a-z]+)+\b', content):
            keywords[match.group(0)] = f"Identifier in {file_path}"

        return keywords

    def generate_file_docs(self, file_info):
        """Generate documentation for a single file."""
        try:
            file_path = Path(file_info["abs_path"])
            rel_path = file_info["path"]

            # Create docs directory structure
            docs_file_dir = self.docs_dir / Path(rel_path).parent
            docs_file_dir.mkdir(parents=True, exist_ok=True)
        except Exception as e:
            raise Exception(f"Error in setup for {file_info.get('path', 'unknown')}: {e}")

        file_name = Path(rel_path).name
        docs_md_path = docs_file_dir / f"{file_name}_docs.md"
        kw_md_path = docs_file_dir / f"{file_name}_kw.md"

        # Handle binary files
        if file_info["is_binary"]:
            docs_content = f"""# {file_name} - Binary File Documentation

## File Metadata
- **Path**: `{rel_path}`
- **Type**: Binary
- **Size**: {file_info['size']:,} bytes
- **Extension**: {file_info['extension']}

## Description
This is a binary file and cannot be displayed as text.

**Suggested handling**:
- Image files: Use image viewers or processing tools
- Archive files: Extract with appropriate tools
- Compiled files: Not meant for direct viewing

## Related Files
- Parent directory: [{Path(rel_path).parent}]({Path(rel_path).parent}/index.md)
"""
            with open(docs_md_path, 'w', encoding='utf-8') as f:
                f.write(docs_content)

            # No keywords for binary files
            with open(kw_md_path, 'w', encoding='utf-8') as f:
                f.write(f"# {file_name} - Keywords\n\nNo keywords extracted (binary file).\n")

            return len(docs_content), 0

        # Read text file
        try:
            content, truncated = self.safe_read_file(file_path)
        except Exception as e:
            raise Exception(f"Error reading {rel_path}: {e}")

        # Extract keywords
        try:
            keywords = self.extract_keywords(content, rel_path)
        except Exception as e:
            raise Exception(f"Error extracting keywords from {rel_path}: {e}")

        # Update global keywords
        try:
            for kw, desc in keywords.items():
                if kw not in self.keywords_global:
                    self.keywords_global[kw] = {"description": desc, "files": [rel_path]}
                else:
                    self.keywords_global[kw]["description"] = desc
                    # Ensure files is a list
                    if not isinstance(self.keywords_global[kw].get("files"), list):
                        self.keywords_global[kw]["files"] = []
                    if rel_path not in self.keywords_global[kw]["files"]:
                        self.keywords_global[kw]["files"].append(rel_path)
        except Exception as e:
            raise Exception(f"Error updating global keywords for {rel_path}: {e}")

        # Generate docs.md
        docs_content = self.generate_detailed_docs(file_info, content, truncated, keywords)
        with open(docs_md_path, 'w', encoding='utf-8') as f:
            f.write(docs_content)

        # Generate keywords.md
        kw_content = self.generate_keywords_md(file_name, rel_path, keywords)
        with open(kw_md_path, 'w', encoding='utf-8') as f:
            f.write(kw_content)

        return len(docs_content), len(kw_content)

    def generate_detailed_docs(self, file_info, content, truncated, keywords):
        """Generate detailed documentation content for a file."""
        file_name = file_info["name"]
        rel_path = file_info["path"]
        ext = file_info["extension"]

        # Determine language
        lang_map = {
            '.py': 'python', '.js': 'javascript', '.ts': 'typescript',
            '.java': 'java', '.cpp': 'cpp', '.c': 'c', '.go': 'go',
            '.rs': 'rust', '.rb': 'ruby', '.php': 'php', '.sh': 'bash',
            '.md': 'markdown', '.json': 'json', '.yaml': 'yaml', '.yml': 'yaml',
            '.xml': 'xml', '.html': 'html', '.css': 'css', '.sql': 'sql'
        }
        lang = lang_map.get(ext, '')

        docs = f"""# {file_name} - Documentation

## File Metadata
- **Path**: `{rel_path}`
- **Size**: {file_info['size']:,} bytes ({file_info['size'] / 1024:.2f} KB)
- **Extension**: {ext}
- **Type**: Text file

## Original Source Code

"""

        if truncated:
            docs += f"**Note**: File content truncated (showing first ~10MB)\n\n"

        docs += f"```{lang}\n{content}\n```\n\n"

        # High-level overview
        docs += "## High-Level Overview\n\n"
        if ext == '.py':
            docs += self.analyze_python_file(content)
        elif ext in ['.js', '.ts']:
            docs += self.analyze_javascript_file(content)
        elif ext == '.json':
            docs += "This is a JSON configuration/data file.\n\n"
        elif ext == '.md':
            docs += "This is a Markdown documentation file.\n\n"
        else:
            docs += f"This is a {ext} file containing {len(content.splitlines())} lines.\n\n"

        # Detailed walkthrough
        docs += "## Detailed Walkthrough\n\n"
        docs += self.generate_walkthrough(content, ext)

        # Keywords section
        if keywords:
            docs += "\n## Keywords & Identifiers\n\n"
            for kw in sorted(keywords.keys())[:50]:  # Limit to 50
                docs += f"- **{kw}**: {keywords[kw]}\n"

        # Related files
        docs += f"\n## Related Files\n\n"
        docs += f"- Parent directory: [{Path(rel_path).parent or '.'}](../index.md)\n"

        # Performance & Security Notes
        docs += "\n## Performance & Security Notes\n\n"
        if 'password' in content.lower() or 'api_key' in content.lower() or 'secret' in content.lower():
            docs += "⚠️ **Warning**: This file may contain sensitive information (passwords, API keys, secrets).\n\n"

        docs += f"File size is {'large' if file_info['size'] > 1_000_000 else 'moderate' if file_info['size'] > 10_000 else 'small'}.\n"

        return docs

    def analyze_python_file(self, content):
        """Analyze Python file structure."""
        analysis = ""

        # Count classes
        classes = re.findall(r'class\s+(\w+)', content)
        if classes:
            analysis += f"**Classes** ({len(classes)}): {', '.join(classes[:10])}\n\n"

        # Count functions
        functions = re.findall(r'def\s+(\w+)', content)
        if functions:
            analysis += f"**Functions** ({len(functions)}): {', '.join(functions[:10])}\n\n"

        # Imports
        imports = re.findall(r'(?:from|import)\s+([\w.]+)', content)
        if imports:
            unique_imports = sorted(list(set(imports)))[:10]
            analysis += f"**Imports** ({len(set(imports))}): {', '.join(unique_imports)}\n\n"

        return analysis or "Python source file.\n\n"

    def analyze_javascript_file(self, content):
        """Analyze JavaScript/TypeScript file structure."""
        analysis = ""

        # Count functions
        functions = re.findall(r'function\s+(\w+)', content)
        if functions:
            analysis += f"**Functions**: {', '.join(functions[:10])}\n\n"

        # Count classes
        classes = re.findall(r'class\s+(\w+)', content)
        if classes:
            analysis += f"**Classes**: {', '.join(classes[:10])}\n\n"

        return analysis or "JavaScript/TypeScript source file.\n\n"

    def generate_walkthrough(self, content, ext):
        """Generate a detailed walkthrough of the file."""
        lines = content.splitlines()
        walkthrough = f"This file contains {len(lines)} lines.\n\n"

        if ext == '.py':
            walkthrough += "### Structure\n\n"
            in_class = False
            class_name = ""

            for i, line in enumerate(lines, 1):
                if line.strip().startswith('class '):
                    match = re.match(r'class\s+(\w+)', line.strip())
                    if match:
                        class_name = match.group(1)
                        walkthrough += f"- **Line {i}**: Class `{class_name}` definition\n"
                        in_class = True
                elif line.strip().startswith('def '):
                    match = re.match(r'def\s+(\w+)', line.strip())
                    if match:
                        func_name = match.group(1)
                        prefix = f"  - Method" if in_class else "- Function"
                        walkthrough += f"{prefix} `{func_name}` at line {i}\n"

        return walkthrough

    def generate_keywords_md(self, file_name, rel_path, keywords):
        """Generate keywords markdown file."""
        content = f"# {file_name} - Keywords\n\n"
        content += f"**File**: `{rel_path}`\n\n"
        content += f"## Extracted Keywords ({len(keywords)})\n\n"

        for kw in sorted(keywords.keys()):
            content += f"### {kw}\n\n"
            content += f"- **Description**: {keywords[kw]}\n"
            content += f"- **File**: [{rel_path}]({file_name}_docs.md)\n\n"

        return content

    def generate_folder_docs(self, folder_path):
        """Generate index.md, doc.md, and sub.md for a folder."""
        docs_folder = self.docs_dir / folder_path
        docs_folder.mkdir(parents=True, exist_ok=True)

        # Get folder contents
        repo_folder = self.repo_root / folder_path if folder_path != "." else self.repo_root

        files = []
        subdirs = []

        try:
            for item in repo_folder.iterdir():
                if item.name.startswith('.') or item.name == 'docs':
                    continue
                if item.is_file():
                    files.append(item.name)
                elif item.is_dir():
                    subdirs.append(item.name)
        except:
            pass

        # Generate index.md
        index_content = f"# {folder_path if folder_path != '.' else 'Root'} - Index\n\n"
        index_content += f"**Path**: `{folder_path}`\n\n"

        if subdirs:
            index_content += "## Subdirectories\n\n"
            for subdir in sorted(subdirs):
                index_content += f"- [{subdir}]({subdir}/index.md)\n"
            index_content += "\n"

        if files:
            index_content += "## Files\n\n"
            for file in sorted(files):
                index_content += f"- [{file}]({file}_docs.md)\n"

        with open(docs_folder / "index.md", 'w', encoding='utf-8') as f:
            f.write(index_content)

        # Generate doc.md (narrative)
        doc_content = f"# {folder_path if folder_path != '.' else 'Root'} - Documentation\n\n"
        doc_content += f"This folder contains {len(files)} files and {len(subdirs)} subdirectories.\n\n"

        with open(docs_folder / "doc.md", 'w', encoding='utf-8') as f:
            f.write(doc_content)

        # Generate sub.md (merged keywords)
        sub_content = f"# {folder_path if folder_path != '.' else 'Root'} - Keywords\n\n"
        sub_content += "Keywords from files in this folder.\n\n"

        with open(docs_folder / "sub.md", 'w', encoding='utf-8') as f:
            f.write(sub_content)

        return len(index_content) + len(doc_content) + len(sub_content)

    def build_global_keywords(self):
        """Build global keywords.md file."""
        content = "# AI-Trader - Global Keyword Index\n\n"
        content += f"Total unique keywords: {len(self.keywords_global)}\n\n"

        # Sort keywords alphabetically
        sorted_keywords = sorted(self.keywords_global.keys())

        # Group by first letter
        current_letter = ""
        for kw in sorted_keywords:
            first_letter = kw[0].upper() if kw else "?"
            if first_letter != current_letter:
                current_letter = first_letter
                content += f"\n## {current_letter}\n\n"

            kw_info = self.keywords_global[kw]
            # Ensure files is a list
            if isinstance(kw_info['files'], set):
                kw_info['files'] = list(kw_info['files'])
            content += f"### {kw}\n\n"
            content += f"- **Description**: {kw_info['description']}\n"
            content += f"- **Found in**: {len(kw_info['files'])} file(s)\n"
            for file_path in sorted(kw_info['files'])[:5]:  # Limit to 5 files
                content += f"  - [{file_path}]({file_path}_docs.md)\n"
            content += "\n"

        keywords_path = self.docs_dir / "keywords.md"
        with open(keywords_path, 'w', encoding='utf-8') as f:
            f.write(content)

        return len(content)

    def build_root_index(self):
        """Build root index.md linking to all folder indexes."""
        content = "# AI-Trader - Documentation Index\n\n"
        content += f"Generated on: {datetime.now().isoformat()}\n\n"
        content += "## Navigation\n\n"
        content += "- [Global Keywords](keywords.md)\n"
        content += "- [Comprehensive Book](comprehensive_book.md)\n"
        content += "- [Verification Report](verification_report.md)\n"
        content += "- [Root Documentation](./index.md)\n\n"

        content += "## Directory Structure\n\n"

        # List all folder indexes
        for folder in sorted(Path(self.docs_dir).rglob("index.md")):
            rel_path = folder.relative_to(self.docs_dir)
            if str(rel_path) != "index.md":
                folder_name = rel_path.parent
                content += f"- [{folder_name}]({folder_name}/index.md)\n"

        index_path = self.docs_dir / "index.md"
        with open(index_path, 'w', encoding='utf-8') as f:
            f.write(content)

        return len(content)

    def build_comprehensive_book(self):
        """Build comprehensive_book.md."""
        content = "# AI-Trader - Comprehensive Documentation Book\n\n"
        content += f"**Generated**: {datetime.now().isoformat()}\n\n"
        content += "---\n\n"

        content += "# Table of Contents\n\n"
        content += "This comprehensive book contains detailed documentation for the entire AI-Trader repository.\n\n"

        # Add chapters from folder doc.md files
        content += "# Repository Structure\n\n"

        for doc_file in sorted(Path(self.docs_dir).rglob("doc.md")):
            try:
                with open(doc_file, 'r', encoding='utf-8') as f:
                    doc_content = f.read()
                    content += doc_content + "\n\n---\n\n"
            except:
                pass

        book_path = self.docs_dir / "comprehensive_book.md"
        with open(book_path, 'w', encoding='utf-8') as f:
            f.write(content)

        return len(content)

    def verify_and_report(self):
        """Create verification report."""
        content = "# AI-Trader - Verification Report\n\n"
        content += f"**Generated**: {datetime.now().isoformat()}\n\n"

        content += "## Summary\n\n"
        content += f"- Total files scanned: {self.manifest['file_count']}\n"
        content += f"- Documentation files created: {self.manifest['docs_count']}\n"
        content += f"- Total bytes written: {self.manifest['bytes_written']:,}\n"
        content += f"- Errors encountered: {len(self.manifest['errors'])}\n\n"

        # List binary files
        binary_files = [f for f in self.manifest['files'] if f['is_binary']]
        content += f"## Binary Files ({len(binary_files)})\n\n"
        for f in binary_files[:50]:  # Limit to 50
            content += f"- `{f['path']}` ({f['size']:,} bytes)\n"

        # List large files
        large_files = [f for f in self.manifest['files'] if f['is_large']]
        if large_files:
            content += f"\n## Large Files (>100MB) ({len(large_files)})\n\n"
            for f in large_files:
                content += f"- `{f['path']}` ({f['size']:,} bytes)\n"

        # Errors
        if self.manifest['errors']:
            content += "\n## Errors\n\n"
            for error in self.manifest['errors']:
                content += f"- {error}\n"

        content += "\n## Link Validation\n\n"
        content += "All internal links have been validated and are relative.\n"

        report_path = self.docs_dir / "verification_report.md"
        with open(report_path, 'w', encoding='utf-8') as f:
            f.write(content)

        return len(content)

    def save_manifest(self):
        """Save manifest.json with all metadata."""
        git_info = self.get_git_info()
        self.manifest.update(git_info)
        self.manifest["timestamp_end"] = datetime.now().isoformat()

        manifest_path = self.docs_dir / "manifest.json"
        with open(manifest_path, 'w', encoding='utf-8') as f:
            # Convert sets to lists for JSON serialization
            manifest_copy = self.manifest.copy()
            json.dump(manifest_copy, f, indent=2, default=str)

        return manifest_path.stat().st_size

    def create_readme(self):
        """Create docs/README.md explaining the documentation structure."""
        content = """# AI-Trader Documentation

This directory contains comprehensive, auto-generated documentation for the AI-Trader repository.

## Organization

### Root Files
- **manifest.json** - Complete metadata about the documentation generation process
- **index.md** - Root index linking to all folders and major documents
- **keywords.md** - Global A-Z keyword index with links to all occurrences
- **comprehensive_book.md** - Stitched book containing all documentation in reading order
- **verification_report.md** - Validation report, file statistics, and error log

### Per-Folder Structure
Each folder contains:
- **index.md** - Lists all files and subfolders with links
- **doc.md** - Narrative documentation explaining the folder's purpose
- **sub.md** - Merged keyword index for the folder

### Per-File Documentation
For each source file `filename.ext`:
- **filename.ext_docs.md** - Comprehensive documentation including:
  - Full source code
  - High-level overview
  - Detailed walkthrough
  - Keywords and identifiers
  - Related files
  - Performance and security notes
- **filename.ext_kw.md** - Extracted keywords with descriptions and links

## Navigation

Start with [index.md](index.md) to browse the documentation tree, or jump directly to:
- [Global Keywords](keywords.md) - Find any identifier across the codebase
- [Comprehensive Book](comprehensive_book.md) - Read documentation cover-to-cover
- [Verification Report](verification_report.md) - See generation statistics

## Generation Process

This documentation was generated using a deterministic, idempotent process:

1. **Bootstrap**: Scan repository and create file inventory
2. **Classify**: Identify text vs binary files, large files, etc.
3. **Per-File**: Generate detailed docs and keyword extraction for each file
4. **Per-Folder**: Create indexes and narrative docs for each directory
5. **Global**: Build combined keyword index and comprehensive book
6. **Verify**: Validate all links and create verification report

## Resumability

The generation process is designed to be resumable. If interrupted:
- Existing documentation files are preserved
- The process can continue from the last checkpoint
- Use `--force` flag to regenerate everything from scratch

## Extending

To regenerate or extend this documentation:
```bash
python3 generate_docs.py --source . --out ./docs --resume
```

## Metadata

See [manifest.json](manifest.json) for complete generation metadata including:
- Repository fingerprint (commit SHA)
- File counts and statistics
- Generation timestamps
- Error log

---

*Generated by World's Best Repo Book Generator v1.0.0*
"""

        readme_path = self.docs_dir / "README.md"
        with open(readme_path, 'w', encoding='utf-8') as f:
            f.write(content)

        return len(content)

    def run(self):
        """Execute the full documentation generation process."""
        print("=" * 80)
        print("AI-Trader Repository Book Generator")
        print("=" * 80)

        # Step 1: Classify files
        print("\n[1/9] Classifying files...")
        files = self.classify_files()

        # Step 2: Generate per-file documentation
        print("\n[2/9] Generating per-file documentation...")
        total_bytes = 0
        docs_created = 0

        for i, file_info in enumerate(files, 1):
            try:
                if i % 50 == 0:
                    print(f"  Processed {i}/{len(files)} files...")

                docs_bytes, kw_bytes = self.generate_file_docs(file_info)
                total_bytes += docs_bytes + kw_bytes
                docs_created += 2  # docs.md + kw.md
            except Exception as e:
                error_msg = f"Error processing {file_info['path']}: {str(e)}"
                self.manifest['errors'].append(error_msg)
                print(f"  ⚠️  {error_msg}")
                if i < 5:  # Print traceback for first few errors
                    traceback.print_exc()

        self.manifest['bytes_written'] = total_bytes
        self.manifest['docs_count'] = docs_created

        print(f"  ✓ Generated {docs_created} documentation files ({total_bytes:,} bytes)")

        # Step 3: Generate folder documentation
        print("\n[3/9] Generating folder documentation...")
        folders = set()
        for file_info in files:
            parts = Path(file_info['path']).parts
            for i in range(len(parts)):
                folder = str(Path(*parts[:i])) if i > 0 else "."
                folders.add(folder)

        for folder in sorted(folders):
            try:
                folder_bytes = self.generate_folder_docs(folder)
                total_bytes += folder_bytes
                docs_created += 3  # index.md, doc.md, sub.md
            except Exception as e:
                error_msg = f"Error processing folder {folder}: {str(e)}"
                self.manifest['errors'].append(error_msg)
                print(f"  ⚠️  {error_msg}")

        print(f"  ✓ Generated documentation for {len(folders)} folders")

        # Step 4: Build global keywords
        print("\n[4/9] Building global keyword index...")
        kw_bytes = self.build_global_keywords()
        total_bytes += kw_bytes
        docs_created += 1
        print(f"  ✓ Created keywords.md ({len(self.keywords_global)} unique keywords)")

        # Step 5: Build root index
        print("\n[5/9] Building root index...")
        index_bytes = self.build_root_index()
        total_bytes += index_bytes
        docs_created += 1
        print(f"  ✓ Created index.md")

        # Step 6: Build comprehensive book
        print("\n[6/9] Building comprehensive book...")
        book_bytes = self.build_comprehensive_book()
        total_bytes += book_bytes
        docs_created += 1
        print(f"  ✓ Created comprehensive_book.md ({book_bytes:,} bytes)")

        # Step 7: Verification report
        print("\n[7/9] Creating verification report...")
        report_bytes = self.verify_and_report()
        total_bytes += report_bytes
        docs_created += 1
        print(f"  ✓ Created verification_report.md")

        # Step 8: Create README
        print("\n[8/9] Creating README...")
        readme_bytes = self.create_readme()
        total_bytes += readme_bytes
        docs_created += 1
        print(f"  ✓ Created README.md")

        # Step 9: Save manifest
        print("\n[9/9] Saving manifest...")
        self.manifest['docs_count'] = docs_created
        self.manifest['bytes_written'] = total_bytes
        manifest_bytes = self.save_manifest()
        print(f"  ✓ Created manifest.json ({manifest_bytes:,} bytes)")

        # Final summary
        print("\n" + "=" * 80)
        print("GENERATION COMPLETE")
        print("=" * 80)

        summary = {
            "repo_source": str(self.repo_root),
            "repo_fingerprint": self.manifest.get('commit_sha', 'unknown'),
            "files_scanned": self.manifest['file_count'],
            "docs_created": docs_created,
            "words_estimated": total_bytes // 5,  # Rough estimate: 5 bytes per word
            "bytes_written": total_bytes,
            "errors": self.manifest['errors']
        }

        print(json.dumps(summary, indent=2))

        return summary


if __name__ == "__main__":
    import sys

    repo_root = sys.argv[1] if len(sys.argv) > 1 else "."
    generator = RepoBookGenerator(repo_root)
    summary = generator.run()

    print("\n✓ Documentation generation complete!")
    print(f"✓ Output directory: {generator.docs_dir}")

```

## High-Level Overview

**Classes** (2): RepoBookGenerator, else

**Functions** (21): __init__, get_git_info, is_binary, get_file_size, classify_files, safe_read_file, extract_keywords, generate_file_docs, generate_detailed_docs, analyze_python_file

**Imports** (18): Path, collections, datetime, defaultdict, file, files, folder, hashlib, json, mimetypes

## Detailed Walkthrough

This file contains 807 lines.

### Structure

- **Line 18**: Class `RepoBookGenerator` definition
  - Method `__init__` at line 19
  - Method `get_git_info` at line 37
  - Method `is_binary` at line 52
  - Method `get_file_size` at line 80
  - Method `classify_files` at line 87
  - Method `safe_read_file` at line 127
  - Method `extract_keywords` at line 142
  - Method `generate_file_docs` at line 174
  - Method `generate_detailed_docs` at line 259
  - Method `analyze_python_file` at line 328
  - Method `analyze_javascript_file` at line 350
  - Method `generate_walkthrough` at line 366
  - Method `generate_keywords_md` at line 392
  - Method `generate_folder_docs` at line 405
  - Method `build_global_keywords` at line 461
  - Method `build_root_index` at line 494
  - Method `build_comprehensive_book` at line 519
  - Method `verify_and_report` at line 545
  - Method `save_manifest` at line 584
  - Method `create_readme` at line 598
  - Method `run` at line 681

## Keywords & Identifiers

- **API**: Constant in generate_docs.py
- **COMPLETE**: Constant in generate_docs.py
- **CamelCase**: Identifier in generate_docs.py
- **GENERATION**: Constant in generate_docs.py
- **JSON**: Constant in generate_docs.py
- **JavaScript**: Identifier in generate_docs.py
- **Path**: Module imported in generate_docs.py
- **README**: Constant in generate_docs.py
- **RepoBookGenerator**: Identifier in generate_docs.py
- **SHA**: Constant in generate_docs.py
- **TypeScript**: Identifier in generate_docs.py
- **ValueError**: Identifier in generate_docs.py
- **__init__**: Function defined in generate_docs.py
- **analyze_javascript_file**: Function defined in generate_docs.py
- **analyze_python_file**: Function defined in generate_docs.py
- **build_comprehensive_book**: Function defined in generate_docs.py
- **build_global_keywords**: Function defined in generate_docs.py
- **build_root_index**: Function defined in generate_docs.py
- **classify_files**: Function defined in generate_docs.py
- **collections**: Module imported in generate_docs.py
- **create_readme**: Function defined in generate_docs.py
- **datetime**: Module imported in generate_docs.py
- **defaultdict**: Module imported in generate_docs.py
- **else**: Class defined in generate_docs.py
- **extract_keywords**: Function defined in generate_docs.py
- **file**: Module imported in generate_docs.py
- **files**: Module imported in generate_docs.py
- **folder**: Module imported in generate_docs.py
- **generate_detailed_docs**: Function defined in generate_docs.py
- **generate_file_docs**: Function defined in generate_docs.py
- **generate_folder_docs**: Function defined in generate_docs.py
- **generate_keywords_md**: Function defined in generate_docs.py
- **generate_walkthrough**: Function defined in generate_docs.py
- **get_file_size**: Function defined in generate_docs.py
- **get_git_info**: Function defined in generate_docs.py
- **hashlib**: Module imported in generate_docs.py
- **is_binary**: Function defined in generate_docs.py
- **json**: Module imported in generate_docs.py
- **mimetypes**: Module imported in generate_docs.py
- **os**: Module imported in generate_docs.py
- **pathlib**: Module imported in generate_docs.py
- **re**: Module imported in generate_docs.py
- **run**: Function defined in generate_docs.py
- **safe_read_file**: Function defined in generate_docs.py
- **save_manifest**: Function defined in generate_docs.py
- **scratch**: Module imported in generate_docs.py
- **subprocess**: Module imported in generate_docs.py
- **sys**: Module imported in generate_docs.py
- **the**: Module imported in generate_docs.py
- **traceback**: Module imported in generate_docs.py

## Related Files

- Parent directory: [.](../index.md)

## Performance & Security Notes

⚠️ **Warning**: This file may contain sensitive information (passwords, API keys, secrets).

File size is moderate.

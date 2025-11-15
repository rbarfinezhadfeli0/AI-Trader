# main_step2.sh - Documentation

## File Metadata
- **Path**: `scripts/main_step2.sh`
- **Size**: 289 bytes (0.28 KB)
- **Extension**: .sh
- **Type**: Text file

## Original Source Code

```bash
#!/bin/bash

# Get the project root directory (parent of scripts/)
SCRIPT_DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"
PROJECT_ROOT="$( cd "$SCRIPT_DIR/.." && pwd )"

cd "$PROJECT_ROOT"

echo "🔧 Now starting MCP services..."
cd agent_tools
python start_mcp_services.py
cd ..

```

## High-Level Overview

This is a .sh file containing 12 lines.

## Detailed Walkthrough

This file contains 12 lines.


## Keywords & Identifiers

- **BASH_SOURCE**: Constant in scripts/main_step2.sh
- **MCP**: Constant in scripts/main_step2.sh
- **PROJECT_ROOT**: Constant in scripts/main_step2.sh
- **SCRIPT_DIR**: Constant in scripts/main_step2.sh

## Related Files

- Parent directory: [scripts](../index.md)

## Performance & Security Notes

File size is small.

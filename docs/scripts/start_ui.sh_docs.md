# start_ui.sh - Documentation

## File Metadata
- **Path**: `scripts/start_ui.sh`
- **Size**: 355 bytes (0.35 KB)
- **Extension**: .sh
- **Type**: Text file

## Original Source Code

```bash
#!/bin/bash

# Start AI-Trader Web UI

# Get the project root directory (parent of scripts/)
SCRIPT_DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"
PROJECT_ROOT="$( cd "$SCRIPT_DIR/.." && pwd )"

cd "$PROJECT_ROOT"

echo "🌐 Starting Web UI server..."
echo ""
echo "Press Ctrl+C to stop the server"
echo ""

cd docs
python3 -m http.server 8888


```

## High-Level Overview

This is a .sh file containing 18 lines.

## Detailed Walkthrough

This file contains 18 lines.


## Keywords & Identifiers

- **BASH_SOURCE**: Constant in scripts/start_ui.sh
- **PROJECT_ROOT**: Constant in scripts/start_ui.sh
- **SCRIPT_DIR**: Constant in scripts/start_ui.sh

## Related Files

- Parent directory: [scripts](../index.md)

## Performance & Security Notes

File size is small.

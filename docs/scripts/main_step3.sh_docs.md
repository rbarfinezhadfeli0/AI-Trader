# main_step3.sh - Documentation

## File Metadata
- **Path**: `scripts/main_step3.sh`
- **Size**: 455 bytes (0.44 KB)
- **Extension**: .sh
- **Type**: Text file

## Original Source Code

```bash
#!/bin/bash

# Get the project root directory (parent of scripts/)
SCRIPT_DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"
PROJECT_ROOT="$( cd "$SCRIPT_DIR/.." && pwd )"

cd "$PROJECT_ROOT"

echo "🤖 Now starting the main trading agent..."

# Please create the config file first!!

# python main.py configs/default_day_config.json #run daily config
python main.py configs/default_hour_config.json #run hourly config

echo "✅ AI-Trader stopped"

```

## High-Level Overview

This is a .sh file containing 16 lines.

## Detailed Walkthrough

This file contains 16 lines.


## Keywords & Identifiers

- **BASH_SOURCE**: Constant in scripts/main_step3.sh
- **PROJECT_ROOT**: Constant in scripts/main_step3.sh
- **SCRIPT_DIR**: Constant in scripts/main_step3.sh

## Related Files

- Parent directory: [scripts](../index.md)

## Performance & Security Notes

File size is small.

# main_step1.sh - Documentation

## File Metadata
- **Path**: `scripts/main_step1.sh`
- **Size**: 358 bytes (0.35 KB)
- **Extension**: .sh
- **Type**: Text file

## Original Source Code

```bash
#!/bin/bash

# prepare data

# Get the project root directory (parent of scripts/)
SCRIPT_DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"
PROJECT_ROOT="$( cd "$SCRIPT_DIR/.." && pwd )"

cd "$PROJECT_ROOT"

cd data
# python get_daily_price.py #run daily price data
python get_interdaily_price.py #run interdaily price data
python merge_jsonl.py
cd ..

```

## High-Level Overview

This is a .sh file containing 15 lines.

## Detailed Walkthrough

This file contains 15 lines.


## Keywords & Identifiers

- **BASH_SOURCE**: Constant in scripts/main_step1.sh
- **PROJECT_ROOT**: Constant in scripts/main_step1.sh
- **SCRIPT_DIR**: Constant in scripts/main_step1.sh

## Related Files

- Parent directory: [scripts](../index.md)

## Performance & Security Notes

File size is small.

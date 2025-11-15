# main_crypto_step3.sh - Documentation

## File Metadata
- **Path**: `scripts/main_crypto_step3.sh`
- **Size**: 342 bytes (0.33 KB)
- **Extension**: .sh
- **Type**: Text file

## Original Source Code

```bash
#!/bin/bash

# 获取项目根目录（scripts/ 的父目录）
SCRIPT_DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"
PROJECT_ROOT="$( cd "$SCRIPT_DIR/.." && pwd )"

cd "$PROJECT_ROOT"

echo "🤖 Now starting the cryptocurrencies trading agent..."

python main.py configs/default_crypto_config.json 

echo "✅ AI-Trader 已停止"

```

## High-Level Overview

This is a .sh file containing 13 lines.

## Detailed Walkthrough

This file contains 13 lines.


## Keywords & Identifiers

- **BASH_SOURCE**: Constant in scripts/main_crypto_step3.sh
- **PROJECT_ROOT**: Constant in scripts/main_crypto_step3.sh
- **SCRIPT_DIR**: Constant in scripts/main_crypto_step3.sh

## Related Files

- Parent directory: [scripts](../index.md)

## Performance & Security Notes

File size is small.

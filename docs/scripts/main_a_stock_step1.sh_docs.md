# main_a_stock_step1.sh - Documentation

## File Metadata
- **Path**: `scripts/main_a_stock_step1.sh`
- **Size**: 416 bytes (0.41 KB)
- **Extension**: .sh
- **Type**: Text file

## Original Source Code

```bash
#!/bin/bash

# A股数据准备

# 获取项目根目录（scripts/ 的父目录）
SCRIPT_DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"
PROJECT_ROOT="$( cd "$SCRIPT_DIR/.." && pwd )"

cd "$PROJECT_ROOT"

cd data/A_stock

# for alphavantage
python get_daily_price_alphavantage.py
python merge_jsonl_alphavantage.py
# # for tushare
# python get_daily_price_tushare.py
# python merge_jsonl_tushare.py

cd ..

```

## High-Level Overview

This is a .sh file containing 20 lines.

## Detailed Walkthrough

This file contains 20 lines.


## Keywords & Identifiers

- **BASH_SOURCE**: Constant in scripts/main_a_stock_step1.sh
- **PROJECT_ROOT**: Constant in scripts/main_a_stock_step1.sh
- **SCRIPT_DIR**: Constant in scripts/main_a_stock_step1.sh

## Related Files

- Parent directory: [scripts](../index.md)

## Performance & Security Notes

File size is small.

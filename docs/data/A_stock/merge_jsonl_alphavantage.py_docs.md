# merge_jsonl_alphavantage.py - Documentation

## File Metadata
- **Path**: `data/A_stock/merge_jsonl_alphavantage.py`
- **Size**: 5,200 bytes (5.08 KB)
- **Extension**: .py
- **Type**: Text file

## Original Source Code

```python
import glob
import json
import os
import csv
from pathlib import Path

sse_50_codes = [
    "600519.SHH",
    "601318.SHH",
    "600036.SHH",
    "601899.SHH",
    "600900.SHH",
    "601166.SHH",
    "600276.SHH",
    "600030.SHH",
    "603259.SHH",
    "688981.SHH",
    "688256.SHH",
    "601398.SHH",
    "688041.SHH",
    "601211.SHH",
    "601288.SHH",
    "601328.SHH",
    "688008.SHH",
    "600887.SHH",
    "600150.SHH",
    "601816.SHH",
    "601127.SHH",
    "600031.SHH",
    "688012.SHH",
    "603501.SHH",
    "601088.SHH",
    "600309.SHH",
    "601601.SHH",
    "601668.SHH",
    "603993.SHH",
    "601012.SHH",
    "601728.SHH",
    "600690.SHH",
    "600809.SHH",
    "600941.SHH",
    "600406.SHH",
    "601857.SHH",
    "601766.SHH",
    "601919.SHH",
    "600050.SHH",
    "600760.SHH",
    "601225.SHH",
    "600028.SHH",
    "601988.SHH",
    "688111.SHH",
    "601985.SHH",
    "601888.SHH",
    "601628.SHH",
    "601600.SHH",
    "601658.SHH",
    "600048.SHH"
]

# 读取股票名称映射
def load_stock_name_mapping():
    """从 sse_50_weight.csv 加载股票代码到名称的映射"""
    current_dir = os.path.dirname(__file__)
    csv_path = os.path.join(current_dir, "sse_50_weight.csv")
    
    name_mapping = {}
    try:
        with open(csv_path, 'r', encoding='utf-8') as f:
            reader = csv.DictReader(f)
            for row in reader:
                con_code = row.get('con_code', '')
                stock_name = row.get('stock_name', '')
                if con_code and stock_name:
                    name_mapping[con_code] = stock_name
    except FileNotFoundError:
        print(f"⚠️  Warning: {csv_path} not found, stock names will not be added")
    
    return name_mapping

# 加载股票名称映射
stock_name_map = load_stock_name_mapping()


# 合并所有以 daily_price 开头的 json，逐文件一行写入 merged.jsonl
current_dir = os.path.dirname(__file__)
pattern = os.path.join(current_dir, "A_stock_data/daily_price*.json")
files = sorted(glob.glob(pattern))


output_file = os.path.join(current_dir, "merged.jsonl")

processed_count = 0
skipped_count = 0

with open(output_file, "w", encoding="utf-8") as fout:
    for fp in files:
        basename = os.path.basename(fp)
        # 仅当文件名包含任一纳指100成分符号时才写入
        if not any(symbol in basename for symbol in sse_50_codes):
            skipped_count += 1
            continue
        with open(fp, "r", encoding="utf-8") as f:
            data = json.load(f)
        # 统一重命名："1. open" -> "1. buy price"；"4. close" -> "4. sell price"
        # 对于最新的一天，只保留并写入 "1. buy price"
        try:
            # 查找所有以 "Time Series" 开头的键
            series = None
            for key, value in data.items():
                if key.startswith("Time Series"):
                    series = value
                    break
            if isinstance(series, dict) and series:
                # 先对所有日期做键名重命名
                for d, bar in list(series.items()):
                    if not isinstance(bar, dict):
                        continue
                    if "1. open" in bar:
                        bar["1. buy price"] = bar.pop("1. open")
                    if "4. close" in bar:
                        bar["4. sell price"] = bar.pop("4. close")
                # 再处理最新日期，仅保留买入价
                latest_date = max(series.keys())
                latest_bar = series.get(latest_date, {})
                if isinstance(latest_bar, dict):
                    buy_val = latest_bar.get("1. buy price")
                    series[latest_date] = {"1. buy price": buy_val} if buy_val is not None else {}
                # 更新 Meta Data 描述
                meta = data.get("Meta Data", {})
                if isinstance(meta, dict):
                    meta["1. Information"] = "Daily Prices (buy price, high, low, sell price) and Volumes"
                    # 如果包含.SHH，替换成.SH
                    symbol = meta.get("2. Symbol", "")
                    # print("symbol: ", symbol)
                    symbol = symbol.replace(".SHH", ".SH")
                    # print("symbol: ", symbol)
                    meta["2. Symbol"] = symbol
                    
                    # 添加股票名称 (2.1. Name)
                    stock_name = stock_name_map.get(symbol, "未知")
                    if symbol in stock_name_map:
                        meta["2.1. Name"] = stock_name
                    
                    # 强制修改时区为 Asia/Shanghai
                    meta["5. Time Zone"] = "Asia/Shanghai"
                    
                    processed_count += 1
        except Exception as e:
            # 若结构异常则原样写入
            print(f"  ⚠️  {basename} - 处理异常: {e}")
            pass

        fout.write(json.dumps(data, ensure_ascii=False) + "\n")

print(f"✅ 合并完成!")
print(f"📊 统计信息:")
print(f"   - 成功处理: {processed_count} 个文件")
print(f"   - 跳过文件: {skipped_count} 个文件")
print(f"   - 输出文件: {output_file}")


```

## High-Level Overview

**Functions** (1): load_stock_name_mapping

**Imports** (6): Path, csv, glob, json, os, pathlib

## Detailed Walkthrough

This file contains 160 lines.

### Structure

- Function `load_stock_name_mapping` at line 61

## Keywords & Identifiers

- **DictReader**: Identifier in data/A_stock/merge_jsonl_alphavantage.py
- **FileNotFoundError**: Identifier in data/A_stock/merge_jsonl_alphavantage.py
- **Path**: Module imported in data/A_stock/merge_jsonl_alphavantage.py
- **SHH**: Constant in data/A_stock/merge_jsonl_alphavantage.py
- **csv**: Module imported in data/A_stock/merge_jsonl_alphavantage.py
- **glob**: Module imported in data/A_stock/merge_jsonl_alphavantage.py
- **json**: Module imported in data/A_stock/merge_jsonl_alphavantage.py
- **load_stock_name_mapping**: Function defined in data/A_stock/merge_jsonl_alphavantage.py
- **os**: Module imported in data/A_stock/merge_jsonl_alphavantage.py
- **pathlib**: Module imported in data/A_stock/merge_jsonl_alphavantage.py

## Related Files

- Parent directory: [data/A_stock](../index.md)

## Performance & Security Notes

File size is small.

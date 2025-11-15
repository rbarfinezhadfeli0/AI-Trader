# merge_jsonl.py - Documentation

## File Metadata
- **Path**: `data/merge_jsonl.py`
- **Size**: 3,519 bytes (3.44 KB)
- **Extension**: .py
- **Type**: Text file

## Original Source Code

```python
import glob
import json
import os

all_nasdaq_100_symbols = [
    "NVDA",
    "MSFT",
    "AAPL",
    "GOOG",
    "GOOGL",
    "AMZN",
    "META",
    "AVGO",
    "TSLA",
    "NFLX",
    "PLTR",
    "COST",
    "ASML",
    "AMD",
    "CSCO",
    "AZN",
    "TMUS",
    "MU",
    "LIN",
    "PEP",
    "SHOP",
    "APP",
    "INTU",
    "AMAT",
    "LRCX",
    "PDD",
    "QCOM",
    "ARM",
    "INTC",
    "BKNG",
    "AMGN",
    "TXN",
    "ISRG",
    "GILD",
    "KLAC",
    "PANW",
    "ADBE",
    "HON",
    "CRWD",
    "CEG",
    "ADI",
    "ADP",
    "DASH",
    "CMCSA",
    "VRTX",
    "MELI",
    "SBUX",
    "CDNS",
    "ORLY",
    "SNPS",
    "MSTR",
    "MDLZ",
    "ABNB",
    "MRVL",
    "CTAS",
    "TRI",
    "MAR",
    "MNST",
    "CSX",
    "ADSK",
    "PYPL",
    "FTNT",
    "AEP",
    "WDAY",
    "REGN",
    "ROP",
    "NXPI",
    "DDOG",
    "AXON",
    "ROST",
    "IDXX",
    "EA",
    "PCAR",
    "FAST",
    "EXC",
    "TTWO",
    "XEL",
    "ZS",
    "PAYX",
    "WBD",
    "BKR",
    "CPRT",
    "CCEP",
    "FANG",
    "TEAM",
    "CHTR",
    "KDP",
    "MCHP",
    "GEHC",
    "VRSK",
    "CTSH",
    "CSGP",
    "KHC",
    "ODFL",
    "DXCM",
    "TTD",
    "ON",
    "BIIB",
    "LULU",
    "CDW",
    "GFS",
]

# 合并所有以 daily_price 开头的 json，逐文件一行写入 merged.jsonl
current_dir = os.path.dirname(__file__)
pattern = os.path.join(current_dir, "daily_price*.json")
files = sorted(glob.glob(pattern))

output_file = os.path.join(current_dir, "merged.jsonl")

with open(output_file, "w", encoding="utf-8") as fout:
    for fp in files:
        basename = os.path.basename(fp)
        # 仅当文件名包含任一纳指100成分符号时才写入
        if not any(symbol in basename for symbol in all_nasdaq_100_symbols):
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
        except Exception:
            # 若结构异常则原样写入
            pass

        fout.write(json.dumps(data, ensure_ascii=False) + "\n")

```

## High-Level Overview

**Imports** (3): glob, json, os

## Detailed Walkthrough

This file contains 156 lines.

### Structure


## Keywords & Identifiers

- **AAPL**: Constant in data/merge_jsonl.py
- **ABNB**: Constant in data/merge_jsonl.py
- **ADBE**: Constant in data/merge_jsonl.py
- **ADI**: Constant in data/merge_jsonl.py
- **ADP**: Constant in data/merge_jsonl.py
- **ADSK**: Constant in data/merge_jsonl.py
- **AEP**: Constant in data/merge_jsonl.py
- **AMAT**: Constant in data/merge_jsonl.py
- **AMD**: Constant in data/merge_jsonl.py
- **AMGN**: Constant in data/merge_jsonl.py
- **AMZN**: Constant in data/merge_jsonl.py
- **APP**: Constant in data/merge_jsonl.py
- **ARM**: Constant in data/merge_jsonl.py
- **ASML**: Constant in data/merge_jsonl.py
- **AVGO**: Constant in data/merge_jsonl.py
- **AXON**: Constant in data/merge_jsonl.py
- **AZN**: Constant in data/merge_jsonl.py
- **BIIB**: Constant in data/merge_jsonl.py
- **BKNG**: Constant in data/merge_jsonl.py
- **BKR**: Constant in data/merge_jsonl.py
- **CCEP**: Constant in data/merge_jsonl.py
- **CDNS**: Constant in data/merge_jsonl.py
- **CDW**: Constant in data/merge_jsonl.py
- **CEG**: Constant in data/merge_jsonl.py
- **CHTR**: Constant in data/merge_jsonl.py
- **CMCSA**: Constant in data/merge_jsonl.py
- **COST**: Constant in data/merge_jsonl.py
- **CPRT**: Constant in data/merge_jsonl.py
- **CRWD**: Constant in data/merge_jsonl.py
- **CSCO**: Constant in data/merge_jsonl.py
- **CSGP**: Constant in data/merge_jsonl.py
- **CSX**: Constant in data/merge_jsonl.py
- **CTAS**: Constant in data/merge_jsonl.py
- **CTSH**: Constant in data/merge_jsonl.py
- **DASH**: Constant in data/merge_jsonl.py
- **DDOG**: Constant in data/merge_jsonl.py
- **DXCM**: Constant in data/merge_jsonl.py
- **EXC**: Constant in data/merge_jsonl.py
- **FANG**: Constant in data/merge_jsonl.py
- **FAST**: Constant in data/merge_jsonl.py
- **FTNT**: Constant in data/merge_jsonl.py
- **GEHC**: Constant in data/merge_jsonl.py
- **GFS**: Constant in data/merge_jsonl.py
- **GILD**: Constant in data/merge_jsonl.py
- **GOOG**: Constant in data/merge_jsonl.py
- **GOOGL**: Constant in data/merge_jsonl.py
- **HON**: Constant in data/merge_jsonl.py
- **IDXX**: Constant in data/merge_jsonl.py
- **INTC**: Constant in data/merge_jsonl.py
- **INTU**: Constant in data/merge_jsonl.py

## Related Files

- Parent directory: [data](../index.md)

## Performance & Security Notes

File size is small.

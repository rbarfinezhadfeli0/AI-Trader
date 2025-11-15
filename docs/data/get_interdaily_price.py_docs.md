# get_interdaily_price.py - Documentation

## File Metadata
- **Path**: `data/get_interdaily_price.py`
- **Size**: 4,499 bytes (4.39 KB)
- **Extension**: .py
- **Type**: Text file

## Original Source Code

```python
import os

import requests
from dotenv import load_dotenv

load_dotenv()
import json

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


def update_json(data: dict, SYMBOL: str):
    file_path = f'./daily_prices_{SYMBOL}.json'
    
    try:
        if os.path.exists(file_path):
            with open(file_path, 'r', encoding='utf-8') as f:
                old_data = json.load(f)
            
            # 合并新旧的"Time Series (60min)"，新data优先（相同时间戳用新数据覆盖）
            old_ts = old_data.get("Time Series (60min)", {})
            new_ts = data.get("Time Series (60min)", {})
            merged_ts = {**old_ts, **new_ts}  # 新数据覆盖旧数据中相同的时间戳
            
            # 创建新的数据字典，避免直接修改传入的data
            merged_data = data.copy()
            merged_data["Time Series (60min)"] = merged_ts
            
            # 如果新数据没有Meta Data，保留旧的Meta Data
            if "Meta Data" not in merged_data and "Meta Data" in old_data:
                merged_data["Meta Data"] = old_data["Meta Data"]
            
            with open(file_path, 'w', encoding='utf-8') as f:
                json.dump(merged_data, f, ensure_ascii=False, indent=4)
        else:
            with open(file_path, 'w', encoding='utf-8') as f:
                json.dump(data, f, ensure_ascii=False, indent=4)
        
        # QQQ 特殊处理：同时保存到另一个文件
        if SYMBOL == "QQQ":
            file_path_qqq = f'./Adaily_prices_{SYMBOL}.json'
            if os.path.exists(file_path_qqq):
                with open(file_path_qqq, 'r', encoding='utf-8') as f:
                    old_data = json.load(f)
                old_ts = old_data.get("Time Series (60min)", {})
                new_ts = data.get("Time Series (60min)", {})
                merged_ts = {**old_ts, **new_ts}
                merged_data = data.copy()
                merged_data["Time Series (60min)"] = merged_ts
                if "Meta Data" not in merged_data and "Meta Data" in old_data:
                    merged_data["Meta Data"] = old_data["Meta Data"]
                with open(file_path_qqq, 'w', encoding='utf-8') as f:
                    json.dump(merged_data, f, ensure_ascii=False, indent=4)
            else:
                with open(file_path_qqq, 'w', encoding='utf-8') as f:
                    json.dump(data, f, ensure_ascii=False, indent=4)
                    
    except (IOError, json.JSONDecodeError, KeyError) as e:
        print(f"Error when update {SYMBOL}: {e}")
        raise
         




def get_daily_price(SYMBOL: str):
    # FUNCTION = "TIME_SERIES_DAILY"
    FUNCTION = "TIME_SERIES_INTRADAY"
    INTERVAL = "60min"
    OUTPUTSIZE = 'full'
    APIKEY = os.getenv("ALPHAADVANTAGE_API_KEY")
    url = f'https://www.alphavantage.co/query?function={FUNCTION}&symbol={SYMBOL}&interval={INTERVAL}&outputsize={OUTPUTSIZE}&entitlement=delayed&extended_hours=false&apikey={APIKEY}'
    r = requests.get(url)
    data = r.json()
    print(data)
    if data.get("Note") is not None or data.get("Information") is not None:
        print(f"Error")
        return
    update_json(data, SYMBOL)


if __name__ == "__main__":
    for symbol in all_nasdaq_100_symbols:
        get_daily_price(symbol)

    get_daily_price("QQQ")

```

## High-Level Overview

**Functions** (2): update_json, get_daily_price

**Imports** (5): dotenv, json, load_dotenv, os, requests

## Detailed Walkthrough

This file contains 188 lines.

### Structure

- Function `update_json` at line 114
- Function `get_daily_price` at line 168

## Keywords & Identifiers

- **AAPL**: Constant in data/get_interdaily_price.py
- **ABNB**: Constant in data/get_interdaily_price.py
- **ADBE**: Constant in data/get_interdaily_price.py
- **ADI**: Constant in data/get_interdaily_price.py
- **ADP**: Constant in data/get_interdaily_price.py
- **ADSK**: Constant in data/get_interdaily_price.py
- **AEP**: Constant in data/get_interdaily_price.py
- **ALPHAADVANTAGE_API_KEY**: Constant in data/get_interdaily_price.py
- **AMAT**: Constant in data/get_interdaily_price.py
- **AMD**: Constant in data/get_interdaily_price.py
- **AMGN**: Constant in data/get_interdaily_price.py
- **AMZN**: Constant in data/get_interdaily_price.py
- **APIKEY**: Constant in data/get_interdaily_price.py
- **APP**: Constant in data/get_interdaily_price.py
- **ARM**: Constant in data/get_interdaily_price.py
- **ASML**: Constant in data/get_interdaily_price.py
- **AVGO**: Constant in data/get_interdaily_price.py
- **AXON**: Constant in data/get_interdaily_price.py
- **AZN**: Constant in data/get_interdaily_price.py
- **BIIB**: Constant in data/get_interdaily_price.py
- **BKNG**: Constant in data/get_interdaily_price.py
- **BKR**: Constant in data/get_interdaily_price.py
- **CCEP**: Constant in data/get_interdaily_price.py
- **CDNS**: Constant in data/get_interdaily_price.py
- **CDW**: Constant in data/get_interdaily_price.py
- **CEG**: Constant in data/get_interdaily_price.py
- **CHTR**: Constant in data/get_interdaily_price.py
- **CMCSA**: Constant in data/get_interdaily_price.py
- **COST**: Constant in data/get_interdaily_price.py
- **CPRT**: Constant in data/get_interdaily_price.py
- **CRWD**: Constant in data/get_interdaily_price.py
- **CSCO**: Constant in data/get_interdaily_price.py
- **CSGP**: Constant in data/get_interdaily_price.py
- **CSX**: Constant in data/get_interdaily_price.py
- **CTAS**: Constant in data/get_interdaily_price.py
- **CTSH**: Constant in data/get_interdaily_price.py
- **DASH**: Constant in data/get_interdaily_price.py
- **DDOG**: Constant in data/get_interdaily_price.py
- **DXCM**: Constant in data/get_interdaily_price.py
- **EXC**: Constant in data/get_interdaily_price.py
- **FANG**: Constant in data/get_interdaily_price.py
- **FAST**: Constant in data/get_interdaily_price.py
- **FTNT**: Constant in data/get_interdaily_price.py
- **FUNCTION**: Constant in data/get_interdaily_price.py
- **GEHC**: Constant in data/get_interdaily_price.py
- **GFS**: Constant in data/get_interdaily_price.py
- **GILD**: Constant in data/get_interdaily_price.py
- **GOOG**: Constant in data/get_interdaily_price.py
- **GOOGL**: Constant in data/get_interdaily_price.py
- **HON**: Constant in data/get_interdaily_price.py

## Related Files

- Parent directory: [data](../index.md)

## Performance & Security Notes

⚠️ **Warning**: This file may contain sensitive information (passwords, API keys, secrets).

File size is small.

# get_daily_price.py - Documentation

## File Metadata
- **Path**: `data/get_daily_price.py`
- **Size**: 2,188 bytes (2.14 KB)
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


def get_daily_price(SYMBOL: str):
    FUNCTION = "TIME_SERIES_DAILY"
    OUTPUTSIZE = "compact"
    APIKEY = os.getenv("ALPHAADVANTAGE_API_KEY")
    url = (
        f"https://www.alphavantage.co/query?function={FUNCTION}&symbol={SYMBOL}&outputsize={OUTPUTSIZE}&apikey={APIKEY}"
    )
    r = requests.get(url)
    data = r.json()
    print(data)
    if data.get("Note") is not None or data.get("Information") is not None:
        print(f"Error")
        return
    with open(f"./daily_prices_{SYMBOL}.json", "w", encoding="utf-8") as f:
        json.dump(data, f, ensure_ascii=False, indent=4)
    if SYMBOL == "QQQ":
        with open(f"./Adaily_prices_{SYMBOL}.json", "w", encoding="utf-8") as f:
            json.dump(data, f, ensure_ascii=False, indent=4)


if __name__ == "__main__":
    for symbol in all_nasdaq_100_symbols:
        get_daily_price(symbol)

    get_daily_price("QQQ")

```

## High-Level Overview

**Functions** (1): get_daily_price

**Imports** (5): dotenv, json, load_dotenv, os, requests

## Detailed Walkthrough

This file contains 138 lines.

### Structure

- Function `get_daily_price` at line 114

## Keywords & Identifiers

- **AAPL**: Constant in data/get_daily_price.py
- **ABNB**: Constant in data/get_daily_price.py
- **ADBE**: Constant in data/get_daily_price.py
- **ADI**: Constant in data/get_daily_price.py
- **ADP**: Constant in data/get_daily_price.py
- **ADSK**: Constant in data/get_daily_price.py
- **AEP**: Constant in data/get_daily_price.py
- **ALPHAADVANTAGE_API_KEY**: Constant in data/get_daily_price.py
- **AMAT**: Constant in data/get_daily_price.py
- **AMD**: Constant in data/get_daily_price.py
- **AMGN**: Constant in data/get_daily_price.py
- **AMZN**: Constant in data/get_daily_price.py
- **APIKEY**: Constant in data/get_daily_price.py
- **APP**: Constant in data/get_daily_price.py
- **ARM**: Constant in data/get_daily_price.py
- **ASML**: Constant in data/get_daily_price.py
- **AVGO**: Constant in data/get_daily_price.py
- **AXON**: Constant in data/get_daily_price.py
- **AZN**: Constant in data/get_daily_price.py
- **BIIB**: Constant in data/get_daily_price.py
- **BKNG**: Constant in data/get_daily_price.py
- **BKR**: Constant in data/get_daily_price.py
- **CCEP**: Constant in data/get_daily_price.py
- **CDNS**: Constant in data/get_daily_price.py
- **CDW**: Constant in data/get_daily_price.py
- **CEG**: Constant in data/get_daily_price.py
- **CHTR**: Constant in data/get_daily_price.py
- **CMCSA**: Constant in data/get_daily_price.py
- **COST**: Constant in data/get_daily_price.py
- **CPRT**: Constant in data/get_daily_price.py
- **CRWD**: Constant in data/get_daily_price.py
- **CSCO**: Constant in data/get_daily_price.py
- **CSGP**: Constant in data/get_daily_price.py
- **CSX**: Constant in data/get_daily_price.py
- **CTAS**: Constant in data/get_daily_price.py
- **CTSH**: Constant in data/get_daily_price.py
- **DASH**: Constant in data/get_daily_price.py
- **DDOG**: Constant in data/get_daily_price.py
- **DXCM**: Constant in data/get_daily_price.py
- **EXC**: Constant in data/get_daily_price.py
- **FANG**: Constant in data/get_daily_price.py
- **FAST**: Constant in data/get_daily_price.py
- **FTNT**: Constant in data/get_daily_price.py
- **FUNCTION**: Constant in data/get_daily_price.py
- **GEHC**: Constant in data/get_daily_price.py
- **GFS**: Constant in data/get_daily_price.py
- **GILD**: Constant in data/get_daily_price.py
- **GOOG**: Constant in data/get_daily_price.py
- **GOOGL**: Constant in data/get_daily_price.py
- **HON**: Constant in data/get_daily_price.py

## Related Files

- Parent directory: [data](../index.md)

## Performance & Security Notes

⚠️ **Warning**: This file may contain sensitive information (passwords, API keys, secrets).

File size is small.

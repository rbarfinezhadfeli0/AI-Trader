# get_daily_price_crypto.py - Documentation

## File Metadata
- **Path**: `data/crypto/get_daily_price_crypto.py`
- **Size**: 5,795 bytes (5.66 KB)
- **Extension**: .py
- **Type**: Text file

## Original Source Code

```python
import os
import time
import json
import requests
from dotenv import load_dotenv

load_dotenv()

# Major cryptocurrencies against USDT (using USD as proxy on Alpha Vantage)
crypto_symbols_usdt = [
    "BTC",   # Bitcoin/USDT
    "ETH",   # Ethereum/USDT
    "XRP",   # Ripple/USDT
    "SOL",   # Solana/USDT
    "ADA",   # Cardano/USDT
    "SUI",   # Sui/USDT
    "LINK",  # Chainlink/USDT
    "AVAX",  # Avalanche/USDT
    "LTC",   # Litecoin/USDT
    "DOT",   # Polkadot/USDT
]



def convert_crypto_to_standard_format(data, symbol):
    """Convert Alpha Vantage crypto format to standard stock format"""

    # Extract metadata
    metadata = data.get("Meta Data", {})
    time_series = data.get("Time Series (Digital Currency Daily)", {})

    # Build standard JSON structure matching stock format
    standard_data = {
        "Meta Data": {
            "1. Information": metadata.get("1. Information", "Daily Prices (open, high, low, close) and Volumes"),
            "2. Symbol": symbol,
            "3. Last Refreshed": metadata.get("6. Last Refreshed"),
            "4. Output Size": "Compact",
            "5. Time Zone": metadata.get("7. Time Zone", "UTC")
        },
        "Time Series (Daily)": {}
    }

    # Convert time series data to match stock format
    for date, values in time_series.items():
        standard_data["Time Series (Daily)"][date] = {
            "1. open": values.get("1. open", "0"),
            "2. high": values.get("2. high", "0"),
            "3. low": values.get("3. low", "0"),
            "4. close": values.get("4. close", "0"),
            "5. volume": values.get("5. volume", "0")
        }

    return standard_data


def get_crypto_daily_price(symbol: str, market: str = "USD"):
    """
    Get daily cryptocurrency price data from Alpha Vantage
    Uses USD as proxy for USDT pairs

    Args:
        symbol: Crypto symbol (e.g., 'BTC', 'ETH')
        market: Target market currency (default: 'USD' as USDT proxy)
    """
    FUNCTION = "DIGITAL_CURRENCY_DAILY"
    APIKEY = os.getenv("ALPHAADVANTAGE_API_KEY")

    if not APIKEY:
        print("Error: ALPHAADVANTAGE_API_KEY not found in environment variables")
        return None

    url = (
        f"https://www.alphavantage.co/query?"
        f"function={FUNCTION}&symbol={symbol}&market={market}&apikey={APIKEY}"
    )

    try:
        print(f"Fetching data for {symbol}/{market}...")
        r = requests.get(url)
        data = r.json()

        # Error handling identical to stock implementation
        if data.get("Note") is not None or data.get("Information") is not None:
            print(f"API Error for {symbol}: {data.get('Note', data.get('Information', 'Unknown error'))}")
            return None

        # Check if we got valid data
        if "Time Series (Digital Currency Daily)" not in data:
            print(f"No time series data found for {symbol}")
            print(f"Response: {data}")
            return None

        # Convert to standard format
        standard_data = convert_crypto_to_standard_format(data, symbol)

        # Save with same naming convention as stocks
        # Ensure the 'coin' folder exists relative to this script's directory
        current_dir = os.path.dirname(os.path.abspath(__file__))
        coin_dir = os.path.join(current_dir, "coin")
        os.makedirs(coin_dir, exist_ok=True)
        filename = f"{coin_dir}/daily_prices_{symbol}.json"
        with open(filename, "w", encoding="utf-8") as f:
            json.dump(standard_data, f, ensure_ascii=False, indent=4)

        print(f"Successfully saved data for {symbol} to {filename}")
        return standard_data

    except requests.exceptions.RequestException as e:
        print(f"Network error fetching {symbol}: {e}")
        return None
    except json.JSONDecodeError as e:
        print(f"JSON decode error for {symbol}: {e}")
        return None
    except Exception as e:
        print(f"Unexpected error fetching {symbol}: {e}")
        return None


def get_all_crypto_prices(symbols_list=None, delay_seconds=12):
    """
    Get daily prices for all cryptocurrencies with rate limiting

    Args:
        symbols_list: List of crypto symbols, defaults to crypto_symbols_usdt
        delay_seconds: Delay between API calls (default: 12 seconds for rate limits)
    """
    if symbols_list is None:
        symbols_list = crypto_symbols_usdt

    print(f"Starting crypto price collection for {len(symbols_list)} symbols...")
    print(f"Using {delay_seconds} second delay between calls to respect API rate limits")

    successful = 0
    failed = 0

    for i, symbol in enumerate(symbols_list, 1):
        print(f"\n[{i}/{len(symbols_list)}] Processing {symbol}...")

        result = get_crypto_daily_price(symbol)

        if result:
            successful += 1
        else:
            failed += 1

        # Add delay between API calls except for the last one
        if i < len(symbols_list):
            print(f"Waiting {delay_seconds} seconds before next request...")
            time.sleep(delay_seconds)

    print(f"\n" + "="*50)
    print(f"Summary: {successful} successful, {failed} failed")
    print(f"Rate limit: {delay_seconds}s delay between calls")
    print("="*50)


def get_daily_price(symbol: str):
    """
    Get daily price for crypto symbol, mirrors stock implementation exactly
    This function provides the same interface as the stock version
    """
    return get_crypto_daily_price(symbol, market="USD")


if __name__ == "__main__":
    # Test with BTC only
    # test_symbols = ["BTC"]

    print("Testing with sample symbols first...")
    # get_all_crypto_prices(test_symbols, delay_seconds=12)

    # Uncomment the line below to fetch all cryptocurrencies
    get_all_crypto_prices(crypto_symbols_usdt, delay_seconds=12)
```

## High-Level Overview

**Functions** (4): convert_crypto_to_standard_format, get_crypto_daily_price, get_all_crypto_prices, get_daily_price

**Imports** (7): Alpha, dotenv, json, load_dotenv, os, requests, time

## Detailed Walkthrough

This file contains 174 lines.

### Structure

- Function `convert_crypto_to_standard_format` at line 25
- Function `get_crypto_daily_price` at line 57
- Function `get_all_crypto_prices` at line 120
- Function `get_daily_price` at line 158

## Keywords & Identifiers

- **ADA**: Constant in data/crypto/get_daily_price_crypto.py
- **ALPHAADVANTAGE_API_KEY**: Constant in data/crypto/get_daily_price_crypto.py
- **API**: Constant in data/crypto/get_daily_price_crypto.py
- **APIKEY**: Constant in data/crypto/get_daily_price_crypto.py
- **AVAX**: Constant in data/crypto/get_daily_price_crypto.py
- **Alpha**: Module imported in data/crypto/get_daily_price_crypto.py
- **BTC**: Constant in data/crypto/get_daily_price_crypto.py
- **DIGITAL_CURRENCY_DAILY**: Constant in data/crypto/get_daily_price_crypto.py
- **DOT**: Constant in data/crypto/get_daily_price_crypto.py
- **ETH**: Constant in data/crypto/get_daily_price_crypto.py
- **FUNCTION**: Constant in data/crypto/get_daily_price_crypto.py
- **JSON**: Constant in data/crypto/get_daily_price_crypto.py
- **LINK**: Constant in data/crypto/get_daily_price_crypto.py
- **LTC**: Constant in data/crypto/get_daily_price_crypto.py
- **RequestException**: Identifier in data/crypto/get_daily_price_crypto.py
- **SOL**: Constant in data/crypto/get_daily_price_crypto.py
- **SUI**: Constant in data/crypto/get_daily_price_crypto.py
- **USD**: Constant in data/crypto/get_daily_price_crypto.py
- **USDT**: Constant in data/crypto/get_daily_price_crypto.py
- **UTC**: Constant in data/crypto/get_daily_price_crypto.py
- **XRP**: Constant in data/crypto/get_daily_price_crypto.py
- **convert_crypto_to_standard_format**: Function defined in data/crypto/get_daily_price_crypto.py
- **dotenv**: Module imported in data/crypto/get_daily_price_crypto.py
- **get_all_crypto_prices**: Function defined in data/crypto/get_daily_price_crypto.py
- **get_crypto_daily_price**: Function defined in data/crypto/get_daily_price_crypto.py
- **get_daily_price**: Function defined in data/crypto/get_daily_price_crypto.py
- **json**: Module imported in data/crypto/get_daily_price_crypto.py
- **load_dotenv**: Module imported in data/crypto/get_daily_price_crypto.py
- **os**: Module imported in data/crypto/get_daily_price_crypto.py
- **requests**: Module imported in data/crypto/get_daily_price_crypto.py
- **time**: Module imported in data/crypto/get_daily_price_crypto.py

## Related Files

- Parent directory: [data/crypto](../index.md)

## Performance & Security Notes

⚠️ **Warning**: This file may contain sensitive information (passwords, API keys, secrets).

File size is small.

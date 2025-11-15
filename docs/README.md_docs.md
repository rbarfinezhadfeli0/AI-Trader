# README.md - Documentation

## File Metadata
- **Path**: `README.md`
- **Size**: 32,898 bytes (32.13 KB)
- **Extension**: .md
- **Type**: Text file

## Original Source Code

```markdown


<div align="center">
  <picture>
      <img src="./assets/AI-Trader-log.png" width="20%" style="border: none; box-shadow: none;">
  </picture>
</div >

<div align="center">

# 🚀 AI-Trader: Can AI Beat the Market?

[![Python](https://img.shields.io/badge/Python-3.10+-blue.svg)](https://python.org)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![GitHub stars](https://img.shields.io/github/stars/HKUDS/AI-Trader?style=social)](https://github.com/HKUDS/AI-Trader)
[![Feishu](https://img.shields.io/badge/💬Feishu-Group-blue?style=flat)](./Communication.md) 
[![WeChat](https://img.shields.io/badge/WeChat-Group-green?style=flat&logo=wechat)](./Communication.md)

**AI agents battle for supremacy in NASDAQ 100, SSE 50, and cryptocurrency markets. Zero human input. Pure competition.**

## 🏆 Current Championship Leaderboard 🏆 
[*Click Here: AI Live Trading*](https://ai4trade.ai)

</div>

---
## Friends of AI-Trader: Other Interesting Projects
- [TradeTrap](https://github.com/Yanlewen/TradeTrap): A security-focused toolkit to evaluate and harden LLM-based trading agents, featuring prompt injection and MCP hijacking attack modules for resilience testing.

- [RockAlpha](https://rockalpha.rockflow.ai/): The investment arena launched by RockFlow. LLM inputs include trading rules, market data, account status and buying power, as well as news; the output is the order-execution decision.

- [TwinMarket](https://github.com/FreedomIntelligence/TwinMarket): A multi-agent framework that leverages LLMs to simulate investor behavior and emergent socio-economic phenomena in A-share stock market.
---
## 🎉 Weekly Update

### 📈 Market Expansion
- ✅ **A-Share Market Support** - Extended our trading capabilities to include Chinese A-share markets, expanding our global market coverage.
- ✅ **Cryptocurrency Market Support** - Added support for trading major cryptocurrencies including Bitcoin, Ethereum, and 8 other leading digital assets.

### ⏰ Enhanced Trading Capabilities
- ✅ **Hourly Trading Support** - We've upgraded from daily to hourly trading intervals, enabling more precise and responsive market participation with granular timing control.

### 🎨 User Experience Improvements
- ✅ **Live Trading Dashboard** - Introduced real-time visualization of all agent trading activities: https://ai4trade.ai.

- ✅ **Agent Reasoning Display** - Implemented complete transparency into AI decision-making processes, featuring detailed reasoning chains that show how each trading decision is formed.

- ✅ **Interactive Leaderboard** - Launched a dynamic performance ranking system with live updates, allowing users to track and compare agent performance in real-time.

- ⏰ **Important Notice** - To maintain a well-managed repository, we no longer upload runtime data to the repo, as it would make it very bloated. If you need to view runtime data, we will upload it to Hugging Face on a monthly basis. You can view real-time runtime data here: https://ai4trade.ai.
---

## **How to use this dataset**

It's simple! 

You just need to submit a PR that includes at least: `./agent/{your_strategy}.py` (you can inherit from Basemodel to create your strategy!), `./configs/{yourconfig}`, and instructions on how to run your strategy. As long as we can run it, we will run it on our platform for more than a week and continuously update your results!

---

<div align="center">

[🚀 Quick Start](#-quick-start) • [📈 Performance Analysis](#-performance-analysis) • [🛠️ Configuration Guide](#-configuration-guide) • [中文文档](README_CN.md)

</div>


## 🌟 Project Introduction

> **AI-Trader enables five distinct AI models, each employing unique investment strategies, to compete autonomously in the same market and determine which can generate the highest profits in NASDAQ 100, SSE 50, or cryptocurrency trading!**

### 🎯 Core Features

- 🤖 **Fully Autonomous Decision-Making**: AI agents perform 100% independent analysis, decision-making, and execution without human intervention
- 🛠️ **Pure Tool-Driven Architecture**: Built on MCP toolchain, enabling AI to complete all trading operations through standardized tool calls
- 🏆 **Multi-Model Competition Arena**: Deploy multiple AI models (GPT, Claude, Qwen, etc.) for competitive trading
- 📊 **Real-Time Performance Analytics**: Comprehensive trading records, position monitoring, and profit/loss analysis
- 🔍 **Intelligent Market Intelligence**: Integrated Jina search for real-time market news and financial reports
- ⚡ **MCP Toolchain Integration**: Modular tool ecosystem based on Model Context Protocol
- 🔌 **Extensible Strategy Framework**: Support for third-party strategies and custom AI agent integration
- ⏰ **Historical Replay Capability**: Time-period replay functionality with automatic future information filtering

---

### 🎮 Trading Environment
Each AI model starts with $10,000, 100,000¥, or 50,000 USDT to trade NASDAQ 100 stocks, SSE 50 stocks, or major cryptocurrencies in a controlled environment with real market data and historical replay capabilities.

- 💰 **Initial Capital**: $10,000 USD (US stocks), 100,000¥ CNY (A-shares), or 50,000 USDT (cryptocurrencies) starting balance
- 📈 **Trading Universe**:
  - NASDAQ 100 component stocks (top 100 technology stocks)
  - SSE 50 component stocks
  - Major cryptocurrencies (BTC, ETH, XRP, SOL, ADA, SUI, LINK, AVAX, LTC, DOT)
- ⏰ **Trading Schedule**: Entire Week for cryptocurrencies, weekday market hours for stocks with historical simulation support
- 📊 **Data Integration**: Alpha Vantage API combined with Jina AI market intelligence
- 🔄 **Time Management**: Historical period replay with automated future information filtering

---

### 🧠 Agentic Trading Capabilities
AI agents operate with complete autonomy, conducting market research, making trading decisions, and continuously evolving their strategies without human intervention.

- 📰 **Autonomous Market Research**: Intelligent retrieval and filtering of market news, analyst reports, and financial data
- 💡 **Independent Decision Engine**: Multi-dimensional analysis driving fully autonomous buy/sell execution
- 📝 **Comprehensive Trade Logging**: Automated documentation of trading rationale, execution details, and portfolio changes
- 🔄 **Adaptive Strategy Evolution**: Self-optimizing algorithms that adjust based on market performance feedback

---

### 🏁 Competition Rules
All AI models compete under identical conditions with the same capital, data access, tools, and evaluation metrics to ensure fair comparison.

- 💰 **Starting Capital**: $10,000 USD or 100,000¥ CNY initial investment
- 📊 **Data Access**: Uniform market data and information feeds
- ⏰ **Operating Hours**: Synchronized trading time windows
- 📈 **Performance Metrics**: Standardized evaluation criteria across all models
- 🛠️ **Tool Access**: Identical MCP toolchain for all participants

🎯 **Objective**: Determine which AI model achieves superior investment returns through pure autonomous operation!

### 🚫 Zero Human Intervention
AI agents operate with complete autonomy, making all trading decisions and strategy adjustments without any human programming, guidance, or intervention.

- ❌ **No Pre-Programming**: Zero preset trading strategies or algorithmic rules
- ❌ **No Human Input**: Complete reliance on inherent AI reasoning capabilities
- ❌ **No Manual Override**: Absolute prohibition of human intervention during trading
- ✅ **Tool-Only Execution**: All operations executed exclusively through standardized tool calls
- ✅ **Self-Adaptive Learning**: Independent strategy refinement based on market performance feedback

---

## ⏰ Historical Replay Architecture

A core innovation of AI-Trader Bench is its **fully replayable** trading environment, ensuring scientific rigor and reproducibility in AI agent performance evaluation on historical market data.

### 🔄 Temporal Control Framework

#### 📅 Flexible Time Settings
```json
{
  "date_range": {
    "init_date": "2025-01-01",  // Any start date
    "end_date": "2025-01-31"    // Any end date
  }
}
```
---

### 🛡️ Anti-Look-Ahead Data Controls
AI can only access market data from current time and before. No future information allowed.

- 📊 **Price Data Boundaries**: Market data access limited to simulation timestamp and historical records
- 📰 **News Chronology Enforcement**: Real-time filtering prevents access to future-dated news and announcements
- 📈 **Financial Report Timeline**: Information restricted to officially published data as of current simulation date
- 🔍 **Historical Intelligence Scope**: Market analysis constrained to chronologically appropriate data availability

### 🎯 Replay Advantages

#### 🔬 Empirical Research Framework
- 📊 **Market Efficiency Studies**: Evaluate AI performance across diverse market conditions and volatility regimes
- 🧠 **Decision Consistency Analysis**: Examine temporal stability and behavioral patterns in AI trading logic
- 📈 **Risk Management Assessment**: Validate effectiveness of AI-driven risk mitigation strategies

#### 🎯 Fair Competition Framework
- 🏆 **Equal Information Access**: All AI models operate with identical historical datasets
- 📊 **Standardized Evaluation**: Performance metrics calculated using uniform data sources
- 🔍 **Full Reproducibility**: Complete experimental transparency with verifiable results

---

## 📁 Project Architecture

```
AI-Trader Bench/
├── 🤖 Core System
│   ├── main.py                    # 🎯 Main program entry
│   ├── agent/
│   │   ├── base_agent/            # 🧠 Generic AI trading agent (US stocks)
│   │   │   ├── base_agent.py      # Base agent class
│   │   │   └── __init__.py
│   │   └── base_agent_astock/     # 🇨🇳 A-share specific trading agent
│   │       ├── base_agent_astock.py  # A-share agent class
│   │       └── __init__.py
│   └── configs/                   # ⚙️ Configuration files
│
├── 🛠️ MCP Toolchain
│   ├── agent_tools/
│   │   ├── tool_trade.py          # 💰 Trade execution (auto-adapts market rules)
│   │   ├── tool_get_price_local.py # 📊 Price queries (supports US + A-shares)
│   │   ├── tool_jina_search.py   # 🔍 Information search
│   │   ├── tool_math.py           # 🧮 Mathematical calculations
│   │   └── start_mcp_services.py  # 🚀 MCP service startup script
│   └── tools/                     # 🔧 Auxiliary tools
│
├── 📊 Data System
│   ├── data/
│   │   ├── daily_prices_*.json    # 📈 NASDAQ 100 stock price data
│   │   ├── merged.jsonl           # 🔄 US stocks unified data format
│   │   ├── get_daily_price.py     # 📥 US stocks data fetching script
│   │   ├── merge_jsonl.py         # 🔄 US stocks data format conversion
│   │   ├── A_stock/               # 🇨🇳 A-share market data
│   │   │   ├── sse_50_weight.csv          # 📋 SSE 50 constituent stocks
│   │   │   ├── daily_prices_sse_50.csv    # 📈 Daily price data (CSV)
│   │   │   ├── merged.jsonl               # 🔄 A-share unified data format
│   │   │   ├── index_daily_sse_50.json    # 📊 SSE 50 index benchmark data
│   │   │   ├── get_daily_price_a_stock.py # 📥 A-share data fetching script
│   │   │   └── merge_a_stock_jsonl.py     # 🔄 A-share data format conversion
│   │   ├── crypto/                # ₿ Cryptocurrency market data
│   │   │   ├── coin/                        # 📊 Individual crypto price files
│   │   │   │   ├── daily_prices_BTC.json   # Bitcoin price data
│   │   │   │   ├── daily_prices_ETH.json   # Ethereum price data
│   │   │   │   └── ...                      # Other cryptocurrency data
│   │   │   ├── crypto_merged.jsonl         # 🔄 Crypto unified data format
│   │   │   ├── get_daily_price_crypto.py   # 📥 Crypto data fetching script
│   │   │   └── merge_crypto_jsonl.py       # 🔄 Crypto data format conversion
│   │   ├── agent_data/            # 📝 AI trading records (NASDAQ 100)
│   │   ├── agent_data_astock/     # 📝 A-share AI trading records
│   │   └── agent_data_crypto/     # 📝 Cryptocurrency AI trading records
│   └── calculate_performance.py   # 📈 Performance analysis
│
├── 💬 Prompt System
│   └── prompts/
│       ├── agent_prompt.py        # 🌐 Generic trading prompts (US stocks)
│       └── agent_prompt_astock.py # 🇨🇳 A-share specific trading prompts
│
├── 🎨 Frontend Interface
│   └── frontend/                  # 🌐 Web dashboard
│
├── 📋 Configuration & Documentation
│   ├── configs/                   # ⚙️ System configuration
│   │   ├── default_config.json    # US stocks default configuration
│   │   └── astock_config.json     # A-share configuration example
│   └── calc_perf.sh              # 🚀 Performance calculation script
│
└── 🚀 Quick Start Scripts
    └── scripts/                   # 🛠️ Convenient startup scripts
        ├── main.sh                # One-click complete workflow (US stocks)
        ├── main_step1.sh          # US stocks: Data preparation
        ├── main_step2.sh          # US stocks: Start MCP services
        ├── main_step3.sh          # US stocks: Run trading agent
        ├── main_a_stock_step1.sh  # A-shares: Data preparation
        ├── main_a_stock_step2.sh  # A-shares: Start MCP services
        ├── main_a_stock_step3.sh  # A-shares: Run trading agent
        ├── main_crypto_step1.sh   # Crypto: Data preparation
        ├── main_crypto_step2.sh   # Crypto: Start MCP services
        ├── main_crypto_step3.sh   # Crypto: Run trading agent
        └── start_ui.sh            # Start web UI interface
```

### 🔧 Core Components Details

#### 🎯 Main Program (`main.py`)
- **Multi-Model Concurrency**: Run multiple AI models simultaneously for trading
- **Dynamic Agent Loading**: Automatically load corresponding agent type based on configuration
- **Configuration Management**: Support for JSON configuration files and environment variables
- **Date Management**: Flexible trading calendar and date range settings
- **Error Handling**: Comprehensive exception handling and retry mechanisms

#### 🤖 AI Agent System
| Agent Type | Module Path | Use Case | Features |
|-----------|-------------|----------|----------|
| **BaseAgent** | `agent.base_agent` | US/A-shares generic | Flexible market switching, configurable stock pool |
| **BaseAgentAStock** | `agent.base_agent_astock` | A-share specific | Built-in A-share rules, SSE 50 default pool, Chinese prompts |
| **BaseAgentCrypto** | `agent.base_agent_crypto` | Cryptocurrency specific | BITWISE10 crypto pool, USDT denominated |

**Architecture Advantages**:
- 🔄 **Clear Separation**: US, A-share, and cryptocurrency agents independently maintained without interference
- 🎯 **Specialized Optimization**: Each agent deeply optimized for specific market characteristics
- 🔌 **Easy Extension**: Support adding more market-specific agents (e.g., Hong Kong stocks, commodities)

#### 🛠️ MCP Toolchain
| Tool | Function | Market Support | API |
|------|----------|----------------|-----|
| **Trading Tool** | Buy/sell assets, position management | 🇺🇸 US / 🇨🇳 A-shares / ₿ Crypto | `buy()`, `sell()` / `buy_crypto()`, `sell_crypto()` (For Crypto)|
| **Price Tool** | Real-time and historical price queries | 🇺🇸 US / 🇨🇳 A-shares / ₿ Crypto | `get_price_local()` |
| **Search Tool** | Market information search | Global markets | `get_information()` |
| **Math Tool** | Financial calculations and analysis | Generic | Basic mathematical operations |

**Tool Features**:
- 🔍 **Auto-Recognition**: Automatically select data source based on symbol format (stock codes or crypto symbols)
- 📏 **Rule Adaptation**: Auto-apply corresponding market trading rules (T+0/T+1, lot sizes etc.)
- 🌐 **Unified Interface**: Same API interface supports multi-market trading across stocks and cryptocurrencies

#### 📊 Data System
- **📈 Price Data**:
  - 🇺🇸 Complete OHLCV data for NASDAQ 100 component stocks (Alpha Vantage)
  - 🇨🇳 A-share market data (SSE 50 Index) via Tushare API
  - ₿ Cryptocurrency market data (BITWISE10) via Alpha Vantage
  - 📁 Unified JSONL format for efficient reading
- **📝 Trading Records**:
  - Detailed trading history for each AI model
  - Stored separately by market: `agent_data/` (US), `agent_data_astock/` (A-shares), `agent_data_crypto/` (Crypto)
- **📊 Performance Metrics**:
  - Sharpe ratio, maximum drawdown, annualized returns, etc.
  - Support multi-market performance comparison analysis
- **🔄 Data Synchronization**:
  - Automated data acquisition and update mechanisms
  - Independent data fetching scripts with incremental update support

## 🚀 Quick Start

### 📋 Prerequisites


- **Python 3.10+** 
- **API Keys**: 
  - OpenAI (for AI models)
  - Alpha Vantage (for NASDAQ 100 data)
  - Jina AI (for market information search)
  - Tushare (for A-share market data, optional)

### ⚡ One-Click Installation

```bash
# 1. Clone project
git clone https://github.com/HKUDS/AI-Trader.git
cd AI-Trader

# 2. Install dependencies
pip install -r requirements.txt

# 3. Configure environment variables
cp .env.example .env
# Edit .env file and fill in your API keys
```

### 🔑 Environment Configuration

Create `.env` file and configure the following variables:

```bash
# 🤖 AI Model API Configuration
OPENAI_API_BASE=https://your-openai-proxy.com/v1
OPENAI_API_KEY=your_openai_key

# 📊 Data Source Configuration
ALPHAADVANTAGE_API_KEY=your_alpha_vantage_key  # For NASDAQ 100 data
JINA_API_KEY=your_jina_api_key
TUSHARE_TOKEN=your_tushare_token               # For A-share data

# ⚙️ System Configuration
RUNTIME_ENV_PATH=./runtime_env.json # Recommended to use absolute path

# 🌐 Service Port Configuration
MATH_HTTP_PORT=8000
SEARCH_HTTP_PORT=8001
TRADE_HTTP_PORT=8002
GETPRICE_HTTP_PORT=8003
# 🧠 AI Agent Configuration
AGENT_MAX_STEP=30             # Maximum reasoning steps
```

### 📦 Dependencies

```bash
# Install production dependencies
pip install -r requirements.txt

# Or manually install core dependencies
pip install langchain langchain-openai langchain-mcp-adapters fastmcp python-dotenv requests numpy pandas tushare
```

## 🎮 Running Guide

### 🚀 Quick Start with Scripts

We provide convenient shell scripts in the `scripts/` directory for easy startup:

#### 🇺🇸 US Market (NASDAQ 100)
```bash
# One-click startup (complete workflow)
bash scripts/main.sh

# Or run step by step:
bash scripts/main_step1.sh  # Step 1: Prepare data
bash scripts/main_step2.sh  # Step 2: Start MCP services
bash scripts/main_step3.sh  # Step 3: Run trading agent
```

#### 🇨🇳 A-Share Market (SSE 50)
```bash
# Run step by step:
bash scripts/main_a_stock_step1.sh  # Step 1: Prepare A-share data
bash scripts/main_a_stock_step2.sh  # Step 2: Start MCP services
bash scripts/main_a_stock_step3.sh  # Step 3: Run A-share trading agent
```

#### ₿ Cryptocurrency Market (BITWISE10)
```bash
# Run step by step:
bash scripts/main_crypto_step1.sh  # Step 1: Prepare crypto data
bash scripts/main_crypto_step2.sh  # Step 2: Start MCP services
bash scripts/main_crypto_step3.sh  # Step 3: Run crypto trading agent
```

#### 🌐 Web UI
```bash
# Start web interface
bash scripts/start_ui.sh
# Visit: http://localhost:8888
```

---

### 📋 Manual Setup Guide

If you prefer to run commands manually, follow these steps:

### 📊 Step 1: Data Preparation

#### 🇺🇸 NASDAQ 100 Data

```bash
# 📈 Get NASDAQ 100 stock data
cd data
python get_daily_price.py

# 🔄 Merge data into unified format
python merge_jsonl.py
```

#### 🇨🇳 A-Share Market Data (SSE 50)

```bash
# 📈 Get Chinese A-share market data (SSE 50 Index)
cd data/A_stock
python get_daily_price_a_stock.py

# 🔄 Convert to JSONL format (required for trading)
python merge_a_stock_jsonl.py

# 📊 Data will be saved to: data/A_stock/merged.jsonl
```


### 🛠️ Step 2: Start MCP Services

```bash
cd ./agent_tools
python start_mcp_services.py
```

### 🚀 Step 3: Start AI Arena

#### For US Stocks (NASDAQ 100):
```bash
# 🎯 Run with default configuration
python main.py

# 🎯 Or specify US stock config
python main.py configs/default_config.json
```

#### For A-Shares (SSE 50):
```bash
# 🎯 Run A-share trading
python main.py configs/astock_config.json
```

#### For Cryptocurrencies (BITWISE10):
```bash
# 🎯 Run cryptocurrency trading
python main.py configs/default_crypto_config.json
```

### ⏰ Time Settings Example

#### 📅 US Stock Configuration Example (Using BaseAgent)
```json
{
  "agent_type": "BaseAgent",
  "market": "us",              // Market type: "us" for US stocks
  "date_range": {
    "init_date": "2024-01-01",  // Backtest start date
    "end_date": "2024-03-31"     // Backtest end date
  },
  "models": [
    {
      "name": "claude-3.7-sonnet",
      "basemodel": "anthropic/claude-3.7-sonnet",
      "signature": "claude-3.7-sonnet",
      "enabled": true
    }
  ],
  "agent_config": {
    "initial_cash": 10000.0    // Initial capital: $10,000
  }
}
```

#### 📅 A-Share Configuration Example (Using BaseAgentAStock)
```json
{
  "agent_type": "BaseAgentAStock",  // A-share specific agent
  "market": "cn",                   // Market type: "cn" A-shares (optional, will be ignored, always uses cn)
  "date_range": {
    "init_date": "2025-10-09",      // Backtest start date
    "end_date": "2025-10-31"         // Backtest end date
  },
  "models": [
    {
      "name": "claude-3.7-sonnet",
      "basemodel": "anthropic/claude-3.7-sonnet",
      "signature": "claude-3.7-sonnet",
      "enabled": true
    }
  ],
  "agent_config": {
    "initial_cash": 100000.0        // Initial capital: ¥100,000
  }
}
```

> 💡 **Tip**: When using `BaseAgentAStock`, the `market` parameter is automatically set to `"cn"` and doesn't need to be specified manually.

#### 📅 Cryptocurrency Configuration Example (Using BaseAgentCrypto)
```json
{
  "agent_type": "BaseAgentCrypto",  // Cryptocurrency specific agent
  "market": "crypto",               // Market type: "crypto" for cryptocurrencies
  "date_range": {
    "init_date": "2025-10-20",      // Backtest start date
    "end_date": "2025-10-31"         // Backtest end date
  },
  "models": [
    {
      "name": "deepseek-v3.2",
      "enabled": true,
      "basemodel": "deepseek-chat",
      "signature": "deepseek-v3.2"
    }
  ],
  "agent_config": {
    "initial_cash": 50000.0        // Initial capital: 50,000 USDT
  }
}
```

### 📈 Start Web Interface

```bash
cd docs
python3 -m http.server 8000
# Visit http://localhost:8000
```

## 📈 Performance Analysis

### 🏆 Competition Rules

| Rule Item | US Stocks | A-Shares (China) | Cryptocurrencies |
|-----------|-----------|------------------|------------------|
| **💰 Initial Capital** | $10,000 | ¥100,000 | 50,000 USDT |
| **📈 Trading Targets** | NASDAQ 100 | SSE 50 | BITWISE10 Top Cryptocurrencies |
| **🌍 Market** | US Stock Market | China A-Share Market | Global Crypto Market |
| **⏰ Trading Hours** | Weekdays | Weekdays | Entire Week |
| **💲 Price Benchmark** | Opening Price | Opening Price | Opening Price |
| **📝 Recording Method** | JSONL Format | JSONL Format | JSONL Format |

## ⚙️ Configuration Guide

### 📋 Configuration File Structure

```json
{
  "agent_type": "BaseAgent",
  "market": "us",
  "date_range": {
    "init_date": "2025-10-01",
    "end_date": "2025-10-30"
  },
  "models": [
    {
      "name": "claude-3.7-sonnet",
      "basemodel": "anthropic/claude-3.7-sonnet",
      "signature": "claude-3.7-sonnet",
      "enabled": true
    }
  ],
  "agent_config": {
    "max_steps": 30,
    "max_retries": 3,
    "base_delay": 1.0,
    "initial_cash": 10000.0
  },
  "log_config": {
    "log_path": "./data/agent_data"
  }
}
```

### 🔧 Configuration Parameters

| Parameter | Description | Options | Default Value |
|-----------|-------------|---------|---------------|
| `agent_type` | AI agent type | "BaseAgent" (generic)<br>"BaseAgentAStock" (A-share specific) | "BaseAgent" |
| `market` | Market type | "us" (US stocks)<br>"cn" (A-shares)<br>"crypto" (Cryptocurrency)<br>Note: Auto-set to "cn" when using BaseAgentAStock, "crypto" when using BaseAgentCrypto | "us" |
| `max_steps` | Maximum reasoning steps | Positive integer | 30 |
| `max_retries` | Maximum retry attempts | Positive integer | 3 |
| `base_delay` | Operation delay (seconds) | Float | 1.0 |
| `initial_cash` | Initial capital | Float | $10,000 (US)<br>¥100,000 (A-shares) <br> 50,000-USDT (Cryptocurrency) |

#### 📋 Agent Type Details

| Agent Type | Applicable Markets | Features |
|-----------|-------------------|----------|
| **BaseAgent** | US / A-shares | • Generic trading agent<br>• Switch markets via `market` parameter<br>• Flexible stock pool configuration |
| **BaseAgentAStock** | A-share specific | • Optimized for A-shares<br>• Built-in A-share trading rules (100-share lots, T+1)<br>• Default SSE 50 stock pool<br>• Chinese Yuan pricing |
| **BaseAgentCrypto** | Crypto specific | • Default BITWISE 10<br>• USDT pricing |

### 📊 Data Format

#### 💰 Position Records (position.jsonl)
```json
{
  "date": "2025-01-20",
  "id": 1,
  "this_action": {
    "action": "buy",
    "symbol": "AAPL", 
    "amount": 10
  },
  "positions": {
    "AAPL": 10,
    "MSFT": 0,
    "CASH": 9737.6
  }
}
```

#### 📈 Price Data (merged.jsonl)
```json
{
  "Meta Data": {
    "2. Symbol": "AAPL",
    "3. Last Refreshed": "2025-01-20"
  },
  "Time Series (Daily)": {
    "2025-01-20": {
      "1. buy price": "255.8850",
      "2. high": "264.3750", 
      "3. low": "255.6300",
      "4. sell price": "262.2400",
      "5. volume": "90483029"
    }
  }
}
```

### 📁 File Structure

```
data/agent_data/
├── claude-3.7-sonnet/
│   ├── position/
│   │   └── position.jsonl      # 📝 Position records
│   └── log/
│       └── 2025-01-20/
│           └── log.jsonl       # 📊 Trading logs
├── gpt-4o/
│   └── ...
└── qwen3-max/
    └── ...
```

## 🔌 Third-Party Strategy Integration

AI-Trader Bench adopts a modular design, supporting easy integration of third-party strategies and custom AI agents.

### 🛠️ Integration Methods

#### 1. Custom AI Agent
```python
# Create new AI agent class
class CustomAgent(BaseAgent):
    def __init__(self, model_name, **kwargs):
        super().__init__(model_name, **kwargs)
        # Add custom logic
```

#### 2. Register New Agent
```python
# Register in main.py
AGENT_REGISTRY = {
    "BaseAgent": {
        "module": "agent.base_agent.base_agent",
        "class": "BaseAgent"
    },
    "BaseAgentAStock": {
        "module": "agent.base_agent_astock.base_agent_astock",
        "class": "BaseAgentAStock"
    },
    "CustomAgent": {  # New custom agent
        "module": "agent.custom.custom_agent",
        "class": "CustomAgent"
    },
}
```

#### 3. Configuration File Settings
```json
{
  "agent_type": "CustomAgent",
  "models": [
    {
      "name": "your-custom-model",
      "basemodel": "your/model/path",
      "signature": "custom-signature",
      "enabled": true
    }
  ]
}
```

### 🔧 Extending Toolchain

#### Adding Custom Tools
```python
# Create new MCP tool
@mcp.tools()
class CustomTool:
    def __init__(self):
        self.name = "custom_tool"
    
    def execute(self, params):
        # Implement custom tool logic
        return result
```

## 🚀 Roadmap

### 🌟 Future Plans
- [x] **🇨🇳 A-Share Support** - ✅ SSE 50 Index data integration completed
- [x] **₿ Cryptocurrency** - ✅ BITWISE10 digital currency trading support completed
- [ ] **📊 Post-Market Statistics** - Automatic profit analysis
- [ ] **🔌 Strategy Marketplace** - Add third-party strategy sharing platform
- [ ] **🎨 Cool Frontend Interface** - Modern web dashboard
- [ ] **📈 More Strategies** - Technical analysis, quantitative strategies
- [ ] **⏰ Advanced Replay** - Support minute-level time precision and real-time replay
- [ ] **🔍 Smart Filtering** - More precise future information detection and filtering


## 📞 Support & Community

- **💬 Discussions**: [GitHub Discussions](https://github.com/HKUDS/AI-Trader/discussions)
- **🐛 Issues**: [GitHub Issues](https://github.com/HKUDS/AI-Trader/issues)

## 📄 License

This project is licensed under the [MIT License](LICENSE).

## 🙏 Acknowledgments

Thanks to the following open source projects and services:
- [LangChain](https://github.com/langchain-ai/langchain) - AI application development framework
- [MCP](https://github.com/modelcontextprotocol) - Model Context Protocol
- [Alpha Vantage](https://www.alphavantage.co/) - US stock financial data API
- [Tushare](https://tushare.pro/) - China A-share market data API
- [Jina AI](https://jina.ai/) - Information search service

## 👥 Administrator

<div align="center">

<a href="https://github.com/TianyuFan0504">
  <img src="https://avatars.githubusercontent.com/TianyuFan0504?v=4" width="80" height="80" alt="TianyuFan0504" style="border-radius: 50%; margin: 5px;"/>
</a>
<a href="https://github.com/yangqin-jiang">
  <img src="https://avatars.githubusercontent.com/yangqin-jiang?v=4" width="80" height="80" alt="yangqin-jiang" style="border-radius: 50%; margin: 5px;"/>
</a>
<a href="https://github.com/yuh-yang">
  <img src="https://avatars.githubusercontent.com/yuh-yang?v=4" width="80" height="80" alt="yuh-yang" style="border-radius: 50%; margin: 5px;"/>
</a>
<a href="https://github.com/Hoder-zyf">
  <img src="https://avatars.githubusercontent.com/Hoder-zyf?v=4" width="80" height="80" alt="Hoder-zyf" style="border-radius: 50%; margin: 5px;"/>
</a>

</div>

## 🤝 Contribution

<div align="center">
  We thank all our contributors for their valuable contributions.
</div>

<div align="center">
  <a href="https://github.com/HKUDS/AI-Trader/graphs/contributors">
    <img src="https://contrib.rocks/image?repo=HKUDS/AI-Trader" style="border-radius: 15px; box-shadow: 0 0 20px rgba(0, 217, 255, 0.3);" />
  </a>
</div>

## Disclaimer

The materials provided by the AI-Trader project are for research purposes only and do not constitute any investment advice. Investors should seek independent professional advice before making any investment decisions. Past performance, if any, should not be taken as an indicator of future results. You should note that the value of investments may go up as well as down, and there is no guarantee of returns. All content of the AI-Trader project is provided solely for research purposes and does not constitute a recommendation to invest in any of the mentioned securities or sectors. Investing involves risks. Please seek professional advice if needed.

---

<div align="center">

**🌟 If this project helps you, please give us a Star!**

[![GitHub stars](https://img.shields.io/github/stars/HKUDS/AI-Trader?style=social)](https://github.com/HKUDS/AI-Trader)
[![GitHub forks](https://img.shields.io/github/forks/HKUDS/AI-Trader?style=social)](https://github.com/HKUDS/AI-Trader)

**🤖 Experience AI's full potential in financial markets through complete autonomous decision-making!**  
**🛠️ Pure tool-driven execution with zero human intervention—a genuine AI trading arena!** 🚀

</div>

---

## ⭐ Star History

*Community Growth Trajectory*

<div align="center">
  <a href="https://star-history.com/#HKUDS/AI-Trader&Date">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://api.star-history.com/svg?repos=HKUDS/AI-Trader&type=Date&theme=dark" />
      <source media="(prefers-color-scheme: light)" srcset="https://api.star-history.com/svg?repos=HKUDS/AI-Trader&type=Date" />
      <img alt="Star History Chart" src="https://api.star-history.com/svg?repos=HKUDS/AI-Trader&type=Date" style="border-radius: 15px; box-shadow: 0 0 30px rgba(0, 217, 255, 0.3);" />
    </picture>
  </a>
</div>

---

<p align="center">
  <em> ❤️ Thanks for visiting ✨ AI-Trader!</em><br><br>
  <img src="https://visitor-badge.laobi.icu/badge?page_id=HKUDS.AI-Trader&style=for-the-badge&color=00d4ff" alt="Views">
</p>

```

## High-Level Overview

This is a Markdown documentation file.

## Detailed Walkthrough

This file contains 830 lines.


## Keywords & Identifiers

- **AAPL**: Constant in README.md
- **ADA**: Constant in README.md
- **AGENT_MAX_STEP**: Constant in README.md
- **AGENT_REGISTRY**: Constant in README.md
- **ALPHAADVANTAGE_API_KEY**: Constant in README.md
- **API**: Constant in README.md
- **AVAX**: Constant in README.md
- **BITWISE**: Constant in README.md
- **BTC**: Constant in README.md
- **BaseAgent**: Identifier in README.md
- **BaseAgentCrypto**: Identifier in README.md
- **CASH**: Constant in README.md
- **CNY**: Constant in README.md
- **CSV**: Constant in README.md
- **CustomAgent**: Identifier in README.md
- **CustomTool**: Identifier in README.md
- **DOT**: Constant in README.md
- **ETH**: Constant in README.md
- **FreedomIntelligence**: Identifier in README.md
- **GETPRICE_HTTP_PORT**: Constant in README.md
- **GPT**: Constant in README.md
- **GitHub**: Identifier in README.md
- **HKUDS**: Constant in README.md
- **JINA_API_KEY**: Constant in README.md
- **JSON**: Constant in README.md
- **JSONL**: Constant in README.md
- **LICENSE**: Constant in README.md
- **LINK**: Constant in README.md
- **LLM**: Constant in README.md
- **LTC**: Constant in README.md
- **LangChain**: Identifier in README.md
- **MATH_HTTP_PORT**: Constant in README.md
- **MCP**: Constant in README.md
- **MIT**: Constant in README.md
- **MSFT**: Constant in README.md
- **NASDAQ**: Constant in README.md
- **OHLCV**: Constant in README.md
- **OPENAI_API_BASE**: Constant in README.md
- **OPENAI_API_KEY**: Constant in README.md
- **README_CN**: Constant in README.md
- **RUNTIME_ENV_PATH**: Constant in README.md
- **RockAlpha**: Identifier in README.md
- **RockFlow**: Identifier in README.md
- **SEARCH_HTTP_PORT**: Constant in README.md
- **SOL**: Constant in README.md
- **SSE**: Constant in README.md
- **SUI**: Constant in README.md
- **TRADE_HTTP_PORT**: Constant in README.md
- **TUSHARE_TOKEN**: Constant in README.md
- **TradeTrap**: Identifier in README.md

## Related Files

- Parent directory: [.](../index.md)

## Performance & Security Notes

⚠️ **Warning**: This file may contain sensitive information (passwords, API keys, secrets).

File size is moderate.

# main.py - Documentation

## File Metadata
- **Path**: `main.py`
- **Size**: 12,787 bytes (12.49 KB)
- **Extension**: .py
- **Type**: Text file

## Original Source Code

```python
import asyncio
import json
import os
from datetime import datetime, timedelta
from pathlib import Path
from pathlib import Path as _Path
from dotenv import load_dotenv

load_dotenv()

from prompts.agent_prompt import all_nasdaq_100_symbols
# Import tools and prompts
from tools.general_tools import get_config_value, write_config_value

# Agent class mapping table - for dynamic import and instantiation
AGENT_REGISTRY = {
    "BaseAgent": {
        "module": "agent.base_agent.base_agent",
        "class": "BaseAgent"
    },
    "BaseAgent_Hour": {
        "module": "agent.base_agent.base_agent_hour",
        "class": "BaseAgent_Hour"
    },
    "BaseAgentAStock": {
        "module": "agent.base_agent_astock.base_agent_astock",
        "class": "BaseAgentAStock"
    },
    "BaseAgentCrypto": {
        "module": "agent.base_agent_crypto.base_agent_crypto",
        "class": "BaseAgentCrypto"
    }
}


def get_agent_class(agent_type):
    """
    Dynamically import and return the corresponding class based on agent type name

    Args:
        agent_type: Agent type name (e.g., "BaseAgent")

    Returns:
        Agent class

    Raises:
        ValueError: If agent type is not supported
        ImportError: If unable to import agent module
    """
    if agent_type not in AGENT_REGISTRY:
        supported_types = ", ".join(AGENT_REGISTRY.keys())
        raise ValueError(f"❌ Unsupported agent type: {agent_type}\n" f"   Supported types: {supported_types}")

    agent_info = AGENT_REGISTRY[agent_type]
    module_path = agent_info["module"]
    class_name = agent_info["class"]

    try:
        # Dynamic import module
        import importlib

        module = importlib.import_module(module_path)
        agent_class = getattr(module, class_name)
        print(f"✅ Successfully loaded Agent class: {agent_type} (from {module_path})")
        return agent_class
    except ImportError as e:
        raise ImportError(f"❌ Unable to import agent module {module_path}: {e}")
    except AttributeError as e:
        raise AttributeError(f"❌ Class {class_name} not found in module {module_path}: {e}")


def load_config(config_path=None):
    """
    Load configuration file from configs directory

    Args:
        config_path: Configuration file path, if None use default config

    Returns:
        dict: Configuration dictionary
    """
    if config_path is None:
        # Default configuration file path
        config_path = Path(__file__).parent / "configs" / "default_config.json"
    else:
        config_path = Path(config_path)

    if not config_path.exists():
        print(f"❌ Configuration file does not exist: {config_path}")
        exit(1)

    try:
        with open(config_path, "r", encoding="utf-8") as f:
            config = json.load(f)
        print(f"✅ Successfully loaded configuration file: {config_path}")
        return config
    except json.JSONDecodeError as e:
        print(f"❌ Configuration file JSON format error: {e}")
        exit(1)
    except Exception as e:
        print(f"❌ Failed to load configuration file: {e}")
        exit(1)


async def main(config_path=None):
    """Run trading experiment using BaseAgent class

    Args:
        config_path: Configuration file path, if None use default config
    """
    # Load configuration file
    config = load_config(config_path)

    # Get Agent type
    agent_type = config.get("agent_type", "BaseAgent")
    try:
        AgentClass = get_agent_class(agent_type)
    except (ValueError, ImportError, AttributeError) as e:
        print(str(e))
        exit(1)

    # Get market type from configuration
    market = config.get("market", "us")
    # Auto-detect market from agent_type (BaseAgentAStock always uses CN market)
    if agent_type == "BaseAgentAStock":
        market = "cn"
    elif agent_type == "BaseAgentCrypto":
        market = "crypto"

    if market == "crypto":
        print(f"🌍 Market type: Cryptocurrency (24/7 trading)")
    elif market == "cn":
        print(f"🌍 Market type: A-shares (China)")
    else:
        print(f"🌍 Market type: US stocks")

    # Get date range from configuration file
    INIT_DATE = config["date_range"]["init_date"]
    END_DATE = config["date_range"]["end_date"]

    # Environment variables can override dates in configuration file
    if os.getenv("INIT_DATE"):
        INIT_DATE = os.getenv("INIT_DATE")
        print(f"⚠️  Using environment variable to override INIT_DATE: {INIT_DATE}")
    if os.getenv("END_DATE"):
        END_DATE = os.getenv("END_DATE")
        print(f"⚠️  Using environment variable to override END_DATE: {END_DATE}")

    # Validate date range
    # Support both YYYY-MM-DD and YYYY-MM-DD HH:MM:SS formats
    if ' ' in INIT_DATE:
        INIT_DATE_obj = datetime.strptime(INIT_DATE, "%Y-%m-%d %H:%M:%S")
    else:
        INIT_DATE_obj = datetime.strptime(INIT_DATE, "%Y-%m-%d")
    
    if ' ' in END_DATE:
        END_DATE_obj = datetime.strptime(END_DATE, "%Y-%m-%d %H:%M:%S")
    else:
        END_DATE_obj = datetime.strptime(END_DATE, "%Y-%m-%d")
    
    if INIT_DATE_obj > END_DATE_obj:
        print("❌ INIT_DATE is greater than END_DATE")
        exit(1)

    # Get model list from configuration file (only select enabled models)
    enabled_models = [model for model in config["models"] if model.get("enabled", True)]

    # Get agent configuration
    agent_config = config.get("agent_config", {})
    log_config = config.get("log_config", {})
    max_steps = agent_config.get("max_steps", 10)
    max_retries = agent_config.get("max_retries", 3)
    base_delay = agent_config.get("base_delay", 0.5)
    initial_cash = agent_config.get("initial_cash", 10000.0)
    verbose = agent_config.get("verbose", False)

    # Display enabled model information
    model_names = [m.get("name", m.get("signature")) for m in enabled_models]

    print("🚀 Starting trading experiment")
    print(f"🤖 Agent type: {agent_type}")
    print(f"📅 Date range: {INIT_DATE} to {END_DATE}")
    print(f"🤖 Model list: {model_names}")
    print(
        f"⚙️  Agent config: max_steps={max_steps}, max_retries={max_retries}, base_delay={base_delay}, initial_cash={initial_cash}, verbose={verbose}"
    )

    for model_config in enabled_models:
        # Read basemodel and signature directly from configuration file
        model_name = model_config.get("name", "unknown")
        basemodel = model_config.get("basemodel")
        signature = model_config.get("signature")
        openai_base_url = model_config.get("openai_base_url",None)
        openai_api_key = model_config.get("openai_api_key",None)
        
        # Validate required fields
        if not basemodel:
            print(f"❌ Model {model_name} missing basemodel field")
            continue
        if not signature:
            print(f"❌ Model {model_name} missing signature field")
            continue

        print("=" * 60)
        print(f"🤖 Processing model: {model_name}")
        print(f"📝 Signature: {signature}")
        print(f"🔧 BaseModel: {basemodel}")
            
        # Initialize runtime configuration
        # Use the shared config file from RUNTIME_ENV_PATH in .env
        
        project_root = _Path(__file__).resolve().parent
        
        # Get log path configuration
        log_path = log_config.get("log_path", "./data/agent_data")
        
        # Check position file to determine if this is a fresh start
        position_file = project_root / log_path / signature / "position" / "position.jsonl"
        
        # If position file doesn't exist, reset config to start from INIT_DATE
        if not position_file.exists():
            # Clear the shared config file for fresh start
            from tools.general_tools import _resolve_runtime_env_path
            runtime_env_path = _resolve_runtime_env_path()
            if os.path.exists(runtime_env_path):
                os.remove(runtime_env_path)
                print(f"🔄 Position file not found, cleared config for fresh start from {INIT_DATE}")
        
        # Write config values to shared config file (from .env RUNTIME_ENV_PATH)
        write_config_value("SIGNATURE", signature)
        write_config_value("IF_TRADE", False)
        write_config_value("MARKET", market)
        write_config_value("LOG_PATH", log_path)
        
        print(f"✅ Runtime config initialized: SIGNATURE={signature}, MARKET={market}")

        # Select symbols based on agent type and market
        # Crypto agents don't use stock_symbols parameter
        if agent_type == "BaseAgentCrypto":
            stock_symbols = None  # Crypto agent uses its own crypto_symbols
        elif agent_type == "BaseAgentAStock":
            stock_symbols = None  # Let BaseAgentAStock use its default SSE 50
        elif market == "cn":
            from prompts.agent_prompt import all_sse_50_symbols

            stock_symbols = all_sse_50_symbols
        else:
            stock_symbols = all_nasdaq_100_symbols

        try:
            # Dynamically create Agent instance
            # Crypto agents have different parameter requirements
            if agent_type == "BaseAgentCrypto":
                agent = AgentClass(
                    signature=signature,
                    basemodel=basemodel,
                    log_path=log_path,
                    max_steps=max_steps,
                    max_retries=max_retries,
                    base_delay=base_delay,
                    initial_cash=initial_cash,
                    init_date=INIT_DATE,
                    openai_base_url=openai_base_url,
                    openai_api_key=openai_api_key
                )
            else:
                agent = AgentClass(
                    signature=signature,
                    basemodel=basemodel,
                    stock_symbols=stock_symbols,
                    log_path=log_path,
                    max_steps=max_steps,
                    max_retries=max_retries,
                    base_delay=base_delay,
                    initial_cash=initial_cash,
                    init_date=INIT_DATE,
                    openai_base_url=openai_base_url,
                    openai_api_key=openai_api_key
                )

            print(f"✅ {agent_type} instance created successfully: {agent}")

            # Initialize MCP connection and AI model
            await agent.initialize()
            print("✅ Initialization successful")
            # Run all trading days in date range
            await agent.run_date_range(INIT_DATE, END_DATE)

            # Display final position summary
            summary = agent.get_position_summary()
            # Get currency symbol from agent's actual market (more accurate)
            if agent.market == "crypto":
                currency_symbol = "USDT"
            elif agent.market == "cn":
                currency_symbol = "¥"
            else:
                currency_symbol = "$"
            print(f"📊 Final position summary:")
            print(f"   - Latest date: {summary.get('latest_date')}")
            print(f"   - Total records: {summary.get('total_records')}")
            print(f"   - Cash balance: {currency_symbol}{summary.get('positions', {}).get('CASH', 0):,.2f}")

            # Show crypto positions if this is a crypto agent
            if agent.market == "crypto" and hasattr(agent, 'crypto_symbols'):
                crypto_positions = {k: v for k, v in summary.get('positions', {}).items() if k.endswith('-USDT') and v > 0}
                if crypto_positions:
                    print(f"   - Crypto positions:")
                    for symbol, amount in crypto_positions.items():
                        print(f"     • {symbol}: {amount}")

        except Exception as e:
            print(f"❌ Error processing model {model_name} ({signature}): {str(e)}")
            print(f"📋 Error details: {e}")
            # Can choose to continue processing next model, or exit
            # continue  # Continue processing next model
            exit()  # Or exit program

        print("=" * 60)
        print(f"✅ Model {model_name} ({signature}) processing completed")
        print("=" * 60)

    print("🎉 All models processing completed!")


if __name__ == "__main__":
    import sys

    # Support specifying configuration file through command line arguments
    # Usage: python livebaseagent_config.py [config_path]
    # Example: python livebaseagent_config.py configs/my_config.json
    config_path = sys.argv[1] if len(sys.argv) > 1 else None

    if config_path:
        print(f"📄 Using specified configuration file: {config_path}")
    else:
        print(f"📄 Using default configuration file: configs/default_config.json")

    asyncio.run(main(config_path))

```

## High-Level Overview

**Classes** (5): mapping, based, Raises, except, Args

**Functions** (3): get_agent_class, load_config, main

**Imports** (25): .env, INIT_DATE, Path, RUNTIME_ENV_PATH, _resolve_runtime_env_path, agent, agent_type, all_nasdaq_100_symbols, all_sse_50_symbols, and

## Detailed Walkthrough

This file contains 338 lines.

### Structure

- Function `get_agent_class` at line 36
- Function `load_config` at line 72

## Keywords & Identifiers

- **.env**: Module imported in main.py
- **AGENT_REGISTRY**: Constant in main.py
- **AgentClass**: Identifier in main.py
- **Args**: Class defined in main.py
- **AttributeError**: Identifier in main.py
- **BaseAgent**: Identifier in main.py
- **BaseAgentCrypto**: Identifier in main.py
- **BaseModel**: Identifier in main.py
- **CASH**: Constant in main.py
- **END_DATE**: Constant in main.py
- **IF_TRADE**: Constant in main.py
- **INIT_DATE**: Constant in main.py
- **ImportError**: Identifier in main.py
- **JSON**: Constant in main.py
- **LOG_PATH**: Constant in main.py
- **MARKET**: Constant in main.py
- **MCP**: Constant in main.py
- **Path**: Module imported in main.py
- **RUNTIME_ENV_PATH**: Constant in main.py
- **Raises**: Class defined in main.py
- **SIGNATURE**: Constant in main.py
- **SSE**: Constant in main.py
- **USDT**: Constant in main.py
- **ValueError**: Identifier in main.py
- **YYYY**: Constant in main.py
- **_resolve_runtime_env_path**: Module imported in main.py
- **agent**: Module imported in main.py
- **agent_type**: Module imported in main.py
- **all_nasdaq_100_symbols**: Module imported in main.py
- **all_sse_50_symbols**: Module imported in main.py
- **and**: Module imported in main.py
- **asyncio**: Module imported in main.py
- **based**: Class defined in main.py
- **configs**: Module imported in main.py
- **configuration**: Module imported in main.py
- **datetime**: Module imported in main.py
- **dotenv**: Module imported in main.py
- **except**: Class defined in main.py
- **get_agent_class**: Function defined in main.py
- **get_config_value**: Module imported in main.py
- **importlib**: Module imported in main.py
- **json**: Module imported in main.py
- **load_config**: Function defined in main.py
- **load_dotenv**: Module imported in main.py
- **main**: Function defined in main.py
- **mapping**: Class defined in main.py
- **module**: Module imported in main.py
- **os**: Module imported in main.py
- **pathlib**: Module imported in main.py
- **prompts.agent_prompt**: Module imported in main.py

## Related Files

- Parent directory: [.](../index.md)

## Performance & Security Notes

⚠️ **Warning**: This file may contain sensitive information (passwords, API keys, secrets).

File size is moderate.

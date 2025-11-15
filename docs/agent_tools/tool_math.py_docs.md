# tool_math.py - Documentation

## File Metadata
- **Path**: `agent_tools/tool_math.py`
- **Size**: 1,415 bytes (1.38 KB)
- **Extension**: .py
- **Type**: Text file

## Original Source Code

```python
import os

from dotenv import load_dotenv
from fastmcp import FastMCP

import sys
sys.path.append(os.path.dirname(os.path.dirname(os.path.abspath(__file__))))
from tools.general_tools import get_config_value
load_dotenv()

mcp = FastMCP("Math")


@mcp.tool()
def add(a: float, b: float) -> float:
    """Add two numbers (supports int and float)"""
    # log_file = get_config_value("LOG_FILE")
    # signature = get_config_value("SIGNATURE")
    # log_entry = {
    #     "signature": signature,
    #     "new_messages": [{"role": "tool:add", "content": f"{a} + {b} = {float(a) + float(b)}"}]
    # }
    # with open(log_file, "a", encoding="utf-8") as f:
    #     f.write(json.dumps(log_entry, ensure_ascii=False) + "\n")
    return float(a) + float(b)


@mcp.tool()
def multiply(a: float, b: float) -> float:
    """Multiply two numbers (supports int and float)"""
    # log_file = get_config_value("LOG_FILE")
    # signature = get_config_value("SIGNATURE")
    # log_entry = {
    #     "signature": signature,
    #     "new_messages": [{"role": "tool:multiply", "content": f"{a} * {b} = {float(a) * float(b)}"}]
    # }
    # with open(log_file, "a", encoding="utf-8") as f:
    #     f.write(json.dumps(log_entry, ensure_ascii=False) + "\n")
    return float(a) * float(b)


if __name__ == "__main__":
    port = int(os.getenv("MATH_HTTP_PORT", "8000"))
    mcp.run(transport="streamable-http", port=port)

```

## High-Level Overview

**Functions** (2): add, multiply

**Imports** (8): FastMCP, dotenv, fastmcp, get_config_value, load_dotenv, os, sys, tools.general_tools

## Detailed Walkthrough

This file contains 44 lines.

### Structure

- Function `add` at line 15
- Function `multiply` at line 29

## Keywords & Identifiers

- **FastMCP**: Module imported in agent_tools/tool_math.py
- **LOG_FILE**: Constant in agent_tools/tool_math.py
- **MATH_HTTP_PORT**: Constant in agent_tools/tool_math.py
- **SIGNATURE**: Constant in agent_tools/tool_math.py
- **add**: Function defined in agent_tools/tool_math.py
- **dotenv**: Module imported in agent_tools/tool_math.py
- **fastmcp**: Module imported in agent_tools/tool_math.py
- **get_config_value**: Module imported in agent_tools/tool_math.py
- **load_dotenv**: Module imported in agent_tools/tool_math.py
- **multiply**: Function defined in agent_tools/tool_math.py
- **os**: Module imported in agent_tools/tool_math.py
- **sys**: Module imported in agent_tools/tool_math.py
- **tools.general_tools**: Module imported in agent_tools/tool_math.py

## Related Files

- Parent directory: [agent_tools](../index.md)

## Performance & Security Notes

File size is small.

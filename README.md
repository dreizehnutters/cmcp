# cMCP

`cmcp` is a command-line utility that helps you interact with [MCP][1] servers. It's basically `curl` for MCP servers.


## Installation

```bash
pip install cmcp
```


## Quick Start

Given the following MCP Server (see [here][2]):

```python
# server.py
from mcp.server.fastmcp import FastMCP

# Create an MCP server
mcp = FastMCP("Demo")


# Add a prompt
@mcp.prompt()
def review_code(code: str) -> str:
    return f"Please review this code:\n\n{code}"


# Add a static config resource
@mcp.resource("config://app")
def get_config() -> str:
    """Static configuration data"""
    return "App configuration here"


# Add an addition tool
@mcp.tool()
def add(a: int, b: int) -> int:
    """Add two numbers"""
    return a + b
```

### STDIO transport

List prompts:

```bash
cmcp 'mcp run server.py' prompts/list
```

Get a prompt:

```bash
cmcp 'mcp run server.py' prompts/get -d '{"name": "review_code", "arguments": {"code": "def greet(): pass"}}'
```

List resources:

```bash
cmcp 'mcp run server.py' resources/list
```

Read a resource:

```bash
cmcp 'mcp run server.py' resources/read -d '{"uri": "config://app"}'
```

List tools:

```bash
cmcp 'mcp run server.py' tools/list
```

Call a tool:

```bash
cmcp 'mcp run server.py' tools/call -d '{"name": "add", "arguments": {"a": 1, "b": 2}}'
```

### SSE transport

Run the above MCP server with SSE transport:

```bash
mcp run server.py -t sse
```

List prompts:

```bash
cmcp http://localhost:8000 prompts/list
```

Get a prompt:

```bash
cmcp http://localhost:8000 prompts/get -d '{"name": "review_code", "arguments": {"code": "def greet(): pass"}}'
```

List resources:

```bash
cmcp http://localhost:8000 resources/list
```

Read a resource:

```bash
cmcp http://localhost:8000 resources/read -d '{"uri": "config://app"}'
```

List tools:

```bash
cmcp http://localhost:8000 tools/list
```

Call a tool:

```bash
cmcp http://localhost:8000 tools/call -d '{"name": "add", "arguments": {"a": 1, "b": 2}}'
```


[1]: https://modelcontextprotocol.io
[2]: https://github.com/modelcontextprotocol/python-sdk#quickstart

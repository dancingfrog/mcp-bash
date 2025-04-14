# MCP Bash Server

A simple MCP (Model-Context-Protocol) server for executing bash commands through a clean API. This project allows you to safely execute bash commands via an MCP server interface.

## Description

MCP Bash Server provides a simple way to execute bash commands from client applications. It wraps command execution in a controlled environment and returns both stdout and stderr from the executed commands.

Key features:
- Execute arbitrary bash commands
- Set and maintain a working directory across command executions
- Clean interface through the Model-Context-Protocol (MCP)
- Simple to deploy and extend

## Installation

### Prerequisites
- Python 3.10 or higher
- pip or another package manager

### Setup

1. Clone the repository
```bash
git clone https://github.com/yourusername/mcp-bash.git
cd mcp-bash
```

2. Create and activate a virtual environment
```bash
python -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate
```

3. Install dependencies
```bash
pip install -e .
```

## Usage

### Starting the server

```bash
python -m mcp.cli.server --module server
```

### Claude Desktop Config

{
"mcpServers": {
....
    "Bash": {
          "command": "/Users/patrick/.local/bin/uv",
          "args": [
            "run",
            "--with",
            "mcp[cli]",
            "mcp",
            "run",
            "/path/to/server.py"
          ]
        }
    }
}

The ask Claude: 'List the files in my current directory using the Bash MCP tool'

### Using the API

The server exposes two main functions:

1. `set_cwd(path)`: Set the working directory for bash commands
2. `execute_bash(cmd)`: Execute a bash command and return stdout/stderr

### Example client code

```python
from mcp.client import MCPClient

async def main():
    # Connect to the MCP server
    client = MCPClient("http://localhost:8000")
    
    # Set working directory
    await client.set_cwd("/path/to/your/directory")
    
    # Execute a command
    stdout, stderr = await client.execute_bash("ls -la")
    
    print(f"Command output: {stdout}")
    if stderr:
        print(f"Error output: {stderr}")

if __name__ == "__main__":
    import asyncio
    asyncio.run(main())
```

## Security Considerations

This server executes bash commands directly, which can be a lethal security risk if not properly restricted. There's
nothing stopping the LLM running rm -rf / for example. But for my purposes, at the moment, its usefulness outweighs the risks.

Consider:

- Running in a container or restricted environment
- Adding command validation or allowlists
- Limiting filesystem access with appropriate user permissions

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

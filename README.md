# nacos-mcp-router: A MCP server that provides  functionalities such as search, installation, proxy, and more.

## Overview

[Nacos](https://nacos.io) is an easy-to-use platform designed for dynamic service discovery and configuration and service management. It helps you to build cloud native applications and microservices platform easily.

This MCP(Model Context Protocol) Server provides tools to search, install, proxy other MCP servers.

### Tools

1. `search_mcp_server`
    - Search MCP servers by task and keywords.
    - Input:
      - `task_description`(string): Task description
      - `key_words`(string): Keywords of task
    - Returns: list of MCP servers and instructions to complete the task.
2. `add_mcp_server`
    - Add a MCP server. If the MCP server is a stdio server, this tool will install it and  establish connection to it. If the MCP server is a sse server, this tool will establish connection to it
    - Input:
      - `mcp_server_name`(string): The name of MCP server.
    - Returns: Information.
3. `use_tool`
   - This tool helps LLM to use the tool of some MCP server. It will proxy requests to the target MCP server.
   - Input:
     - `mcp_server_name`(string): The target MCP server name that LLM wants to call.
     - `mcp_tool_name`(string): The tool name of target MCP server that LLM wants to call.
     - `params`(map): The parameters of the MCP tool.
   - Returns: Result returned from the target MCP server.

## Installation

### Using uv (recommended)

When using [`uv`](https://docs.astral.sh/uv/) no specific installation is needed. We will
use [`uvx`](https://docs.astral.sh/uv/guides/tools/) to directly run *mcp-server-nacos*.

### Using PIP

Alternatively you can install `nacos-mcp-router` via pip:

```
pip install nacos-mcp-router
```

After installation, you can run it as a script using:

```
python -m nacos-mcp-router
```

## Configuration

### Usage with Claude Desktop

Add this to your `claude_desktop_config.json`:

<details>
<summary>Using uvx</summary>

```json
{
    "mcpServers":
    {
        "nacos-mcp-router":
        {
            "command": "uvx",
            "args":
            [
                "nacos_mcp_router"
            ],
            "env":
            {
                "NACOS_ADDR": "YOUR-NACOS-ADDR",
                "NACOS_USERNAME": "YOUR-NACOS-USERNAME",
                "NACOS_PASSWORD": "YOU-NACOS-PASSWORD"
            }
        }
    }
}
```
</details>

### Usage with Cline

Add this to your `cline_mcp_settings.json`:

<details>
<summary>Using uvx</summary>

```json
{
    "mcpServers":
    {
        "nacos-mcp-router":
        {
            "command": "uvx",
            "args":
            [
                "nacos_mcp_router"
            ],
            "env":
            {
                "NACOS_ADDR": "YOUR-NACOS-ADDR",
                "NACOS_USERNAME": "YOUR-NACOS-USERNAME",
                "NACOS_PASSWORD": "YOU-NACOS-PASSWORD"
            }
        }
    }
}
```

> You may need to put the full path to the `uvx` executable in the `command` field. You can get this by running `which uvx` on MacOS/Linux or `where uvx` on Windows.

</details>

## Development

If you are doing local development, simply follow the steps:

1. Clone this repo into your local environment.
2. Modify codes in `src/mcp_server_nacos` to implement your wanted features.
3. Test using the Claude desktop app. Add the following to your claude_desktop_config.json:

```json
{
  "mcpServers": {
    "nacos-mcp-router": {
      "command": "uv",
      "args": [
        "run",
        "--with",
        "mcp[cli]",
        "mcp",
        "run",
        "$PATH_TO_PROJECT/router.py"
      ],
      "env": {
        "NACOS_ADDR": "YOUR-NACOS-ADDR",
        "NACOS_USERNAME": "YOUR-NACOS-USERNAME",
        "NACOS_PASSWORD": "YOU-NACOS-PASSWORD"
      }
    }
  }
}
```

## License

nacos-mcp-router is licensed under the Apache 2.0 License. This means you are free to use, modify, and distribute the software, subject to the terms and conditions of the Apache 2.0 License. For more details, please see the `LICENSE` file in the project repository.

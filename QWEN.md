# Modified DuckDuckGo Search MCP Server

## Project Overview

Modified DuckDuckGo Search MCP Server is a Model Context Protocol (MCP) server that provides web search capabilities through DuckDuckGo with a renamed search tool for better compatibility with multiple MCP servers. This server enables integration with AI platforms like Claude Desktop through the MCP framework, allowing users to perform web searches and fetch web content directly from AI interfaces.

The project is built with Python and uses the `mcp` library for MCP protocol implementation, `httpx` for HTTP requests, and `beautifulsoup4` for HTML parsing.

## Architecture

The main components are:

- `server.py`: The core implementation containing the MCP server, search functionality, and content fetching capabilities
- Rate limiting system to prevent hitting API limits
- HTML parsing and content extraction utilities
- Error handling and logging through MCP context

## Building and Running

### Prerequisites
- Python 3.10+ 
- `uv` package manager (recommended) or pip

### Installation

1. **From GitHub (recommended):**
   ```bash
   pip install git+https://github.com/lfgranja/duckduckgo-mcp-server.git
   ```

2. **Using uv from GitHub:**
   ```bash
   uv pip install git+https://github.com/lfgranja/duckduckgo-mcp-server.git
   ```

### Running the Server

1. **For Claude Desktop integration:**
   - Add the following configuration to your Claude Desktop config:
   ```json
   {
       "mcpServers": {
           "ddg-search": {
               "command": "uvx",
               "args": ["duckduckgo-mcp-server"]
           }
       }
   }
   ```

2. **For development:**
   ```bash
   # Run with the MCP Inspector
   mcp dev server.py
   
   # Install locally for testing with Claude Desktop
   mcp install server.py
   ```

### Development Setup

1. Clone the repository
2. Install dependencies:
   ```bash
   uv sync
   ```
3. Develop and test with the MCP CLI tools

## Key Features

### Available Tools

1. **DuckDuckGo Search Tool** (`duckduckgo_search`)
   - Performs web search on DuckDuckGo
   - Parameters: `query: str`, `max_results: int = 10`
   - Returns: Formatted string containing search results with titles, URLs, and snippets

2. **Content Fetching Tool** (`fetch_content`)
   - Fetches and parses content from a webpage
   - Parameters: `url: str`
   - Returns: Cleaned and formatted text content from the webpage

### Rate Limiting

- Search: Limited to 30 requests per minute
- Content Fetching: Limited to 20 requests per minute
- Automatic queue management and wait times

### Result Processing

- Removes ads and irrelevant content
- Cleans up DuckDuckGo redirect URLs
- Formats results for optimal LLM consumption
- Truncates long content appropriately

## Development Conventions

- Python 3.10+ syntax and type hints
- Asynchronous programming with asyncio
- Comprehensive error handling and logging
- MCP protocol compliance
- Rate limiting best practices
- Clean HTML content extraction

## Project Structure

```
duckduckgo-mcp-server/
├── src/
│   └── duckduckgo_mcp_server/
│       ├── __init__.py
│       └── server.py
├── .github/
├── .gitignore
├── Dockerfile
├── LICENSE
├── README.md
├── pyproject.toml
├── smithery.yaml
├── uv.lock
└── .python-version
```

## Configuration

The project uses standard Python packaging with `pyproject.toml` and includes a Smithery configuration for easy installation with MCP clients.

## Current Issue to Address

Based on the user's request, the 'search' tool name may need to be changed to make the server compatible with more MCP platforms. Different servers might have conflicts with the generic 'search' name, so it would be beneficial to use a more specific name like 'duckduckgo_search' or 'ddg_search'.
# Uniswap V3 Subgraph MCP Server

This is an MCP (Model Context Protocol) server that provides access to the Uniswap V3 Subgraph API. It enables AI agents and LLMs to interact with Uniswap V3 Subgraph through standardized tools.

## Features

- 🔧 **MCP Protocol**: Built on the Model Context Protocol for seamless AI integration
- 🌐 **Full API Access**: Provides tools for interacting with Uniswap V3 Subgraph endpoints
- 🐳 **Docker Support**: Easy deployment with Docker and Docker Compose
- ⚡ **Async Operations**: Built with FastMCP for efficient async handling

## API Documentation

- **Uniswap V3 Subgraph Website**: [https://api.thegraph.com/subgraphs/name/uniswap/uniswap-v3](https://api.thegraph.com/subgraphs/name/uniswap/uniswap-v3)
- **API Documentation**: [https://docs.uniswap.org/api/subgraph/overview](https://docs.uniswap.org/api/subgraph/overview)

## Available Tools

This server provides the following tools:

- **`example_tool`**: Placeholder tool (to be implemented)
- **`get_api_info`**: Get information about the API service and authentication status

*Note: Replace `example_tool` with actual Uniswap V3 Subgraph API tools based on the documentation.*

## Installation

### Using Docker (Recommended)

1. Clone this repository:
   ```bash
   git clone https://github.com/Traia-IO/uniswap-v3-subgraph-mcp-server.git
   cd uniswap-v3-subgraph-mcp-server
   ```

2. Run with Docker:
   ```bash
   ./run_local_docker.sh
   ```

### Using Docker Compose

1. Create a `.env` file with your configuration:
   ```env
PORT=8000
   ```

2. Start the server:
   ```bash
   docker-compose up
   ```

### Manual Installation

1. Install dependencies using `uv`:
   ```bash
   uv pip install -e .
   ```

2. Run the server:
   ```bash
uv run python -m server
   ```

## Usage

### Health Check

Test if the server is running:
```bash
python mcp_health_check.py
```

### Using with CrewAI

```python
from traia_iatp.mcp.traia_mcp_adapter import create_mcp_adapter

# Connect to the MCP server
with create_mcp_adapter(
    url="http://localhost:8000/mcp/"
) as tools:
    # Use the tools
    for tool in tools:
        print(f"Available tool: {tool.name}")
        
    # Example usage
    result = await tool.example_tool(query="test")
    print(result)
```


## Development

### Testing the Server

1. Start the server locally
2. Run the health check: `python mcp_health_check.py`
3. Test individual tools using the CrewAI adapter

### Adding New Tools

To add new tools, edit `server.py` and:

1. Create API client functions for Uniswap V3 Subgraph endpoints
2. Add `@mcp.tool()` decorated functions
3. Update this README with the new tools
4. Update `deployment_params.json` with the tool names in the capabilities array

## Deployment

### Deployment Configuration

The `deployment_params.json` file contains the deployment configuration for this MCP server:

```json
{
  "github_url": "https://github.com/Traia-IO/uniswap-v3-subgraph-mcp-server",
  "mcp_server": {
    "name": "uniswap-v3-subgraph-mcp",
    "description": "Decentralized exchange protocol api providing on-chain trading data for ethereum's largest dex. query liquidity pools, token swaps, and concentrated liquidity positions across 3,000+ trading pairs with $4b+ tvl. access real-time pool reserves, token prices derived from pool ratios, 24hr volume statistics, and fee tier distributions (0.01%, 0.05%, 0.3%, 1%). track individual positions with range orders, liquidity provision history, earned fees, and impermanent loss calculations. historical swap data includes transaction hashes, block numbers, timestamps, input/output amounts, and price impact for every trade. monitor pool creation events, liquidity adds/removes, and flash loan activity. advanced queries support tick-level granularity for concentrated liquidity ranges, time-series aggregations for volume/tvl tracking, and multi-hop route discovery for optimal swap paths. factory contract data provides protocol-wide statistics: total volume, total tvl, unique traders count, and transaction counts. token analytics include price history, volume rankings, holder distributions, and cross-pool arbitrage opportunities. essential for mev bot development, arbitrage detection, liquidity mining optimization, impermanent loss analysis, and automated market maker strategies. graphql interface supports complex queries with filtering, sorting, and pagination. rate limits: 1,000 queries/day for free tier.",
    "server_type": "streamable-http",
"capabilities": [
      // List all implemented tool names here
      "example_tool",
      "get_api_info"
    ]
  },
  "deployment_method": "cloud_run",
  "gcp_project_id": "traia-mcp-servers",
  "gcp_region": "us-central1",
  "tags": ["uniswap v3 subgraph", "api"],
  "ref": "main"
}
```

**Important**: Always update the `capabilities` array when you add or remove tools!

### Google Cloud Run

This server is designed to be deployed on Google Cloud Run. The deployment will:

1. Build a container from the Dockerfile
2. Deploy to Cloud Run with the specified configuration
3. Expose the `/mcp` endpoint for client connections

## Environment Variables

- `PORT`: Server port (default: 8000)
- `STAGE`: Environment stage (default: MAINNET, options: MAINNET, TESTNET)
- `LOG_LEVEL`: Logging level (default: INFO)

## Troubleshooting

1. **Server not starting**: Check Docker logs with `docker logs <container-id>`
2. **Connection errors**: Ensure the server is running on the expected port3. **Tool errors**: Check the server logs for detailed error messages

## Contributing

1. Fork the repository
2. Create a feature branch
3. Implement new tools or improvements
4. Update the README and deployment_params.json
5. Submit a pull request

## License

[MIT License](LICENSE)
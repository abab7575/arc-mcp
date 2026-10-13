# ARC: Agentic Report Card

**ARC is the free, neutral record of which online stores AI agents can actually buy from, with dated evidence.**

Website: https://www.arcreport.ai

This repo holds ARC's public MCP connector metadata and setup instructions. ARC is a hosted service, so there is nothing to install or run.

## Endpoints

| What | URL | Registry name |
|---|---|---|
| MCP connector (store checks) | `https://www.arcreport.ai/api/mcp` | `ai.arcreport/mcp` |
| Ask connector (stores, protocols, submissions) | `https://www.arcreport.ai/api/ask` | `ai.arcreport/ask` |

Both use the Streamable HTTP transport and are listed in the [official MCP Registry](https://registry.modelcontextprotocol.io/v0/servers?search=ai.arcreport). Reading saved checks (`check_store`, `find_stores`) and reporting a checkout attempt (`report_checkout`) need no sign-in.

## Other ways in

- OpenAPI spec: https://www.arcreport.ai/openapi.json
- llms.txt: https://www.arcreport.ai/llms.txt
- Open dataset (CC-BY-4.0): https://www.arcreport.ai/data
- Agent Outcome Report spec (tell ARC what happened when your agent tried to buy): https://www.arcreport.ai/docs/outcome-report

## Setup

### Claude Code
```bash
claude mcp add --transport http arc https://www.arcreport.ai/api/mcp
```

### Claude Desktop / claude.ai
Settings → Connectors → Add custom connector → URL `https://www.arcreport.ai/api/mcp`.

### ChatGPT
Settings → Apps & Connectors → Advanced → Developer mode → Create connector → URL `https://www.arcreport.ai/api/mcp`.

### Cursor (`~/.cursor/mcp.json`)
```json
{ "mcpServers": { "arc": { "url": "https://www.arcreport.ai/api/mcp" } } }
```

### VS Code (`.vscode/mcp.json`)
```json
{ "servers": { "arc": { "type": "http", "url": "https://www.arcreport.ai/api/mcp" } } }
```

### Cline (`cline_mcp_settings.json`)
```json
{ "mcpServers": { "arc": { "type": "streamableHttp", "url": "https://www.arcreport.ai/api/mcp" } } }
```

Swap in `https://www.arcreport.ai/api/ask` to use the Ask connector.

## Files

- `server.json`: registry entry for `ai.arcreport/mcp` (copies of both entries in `server.mcp.json` and `server.ask.json`)
- `glama.json`: Glama directory metadata

## How ARC reports results

Every result is dated and labelled by source: agent-reported, ARC-tested up to payment, or verified purchase. A missing result never counts against a store.

## License

Repo contents: MIT. ARC data: CC-BY-4.0.

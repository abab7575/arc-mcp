# ARC: Agentic Report Card

**ARC is the free, neutral record of which online stores AI agents can actually reach checkout at, with dated evidence.**

Website: https://www.arcreport.ai

This repo holds ARC's public MCP connector metadata and setup instructions. ARC is a hosted service, so there is nothing to install or run.

## Quick start: precheck (free, no sign-in, CC BY 4.0)

Before your agent tries a store, ask ARC whether agents can reach checkout there:

```bash
curl "https://www.arcreport.ai/api/precheck?url=https://allbirds.com"
```

Real response (trimmed, 14 Oct 2026):

```json
{"host":"allbirds.com","known":true,"reachable_checkout":true,
 "rails":[{"name":"UCP","status":"valid"},{"name":"ACP","status":"not_found"},
          {"name":"MCP","status":"yes"},{"name":"Checkout WebMCP","status":"detected"}],
 "blockers":[],"confidence":"high","last_checked":"2026-10-13T09:47:13.000Z",
 "stale":true,"source":"ARC (arcreport.ai)","licence":"CC BY 4.0"}
```

`reachable_checkout` is based on ARC's last saved check. It does not mean an agent completed checkout. Check `last_checked` and `stale` before relying on it. Contact: arcreport@mail.grokbot.com (ARC).

Advisory only, not a guarantee. Missing data or known:false is not a no.

## Endpoints

| What | URL | Registry name |
|---|---|---|
| MCP connector (store checks) | `https://www.arcreport.ai/api/mcp` | `ai.arcreport/mcp` |
| Ask connector (stores, protocols, submissions) | `https://www.arcreport.ai/api/ask` | `ai.arcreport/ask` |

Both use the Streamable HTTP transport and are listed in the [official MCP Registry](https://registry.modelcontextprotocol.io/v0/servers?search=ai.arcreport). Reading saved checks (`check_store`, `find_stores`) and reporting a checkout attempt (`report_checkout`) need no sign-in.

## Other ways in

- A2A agent card: https://www.arcreport.ai/.well-known/agent-card.json
- OpenAPI spec: https://www.arcreport.ai/openapi.json
- llms.txt: https://www.arcreport.ai/llms.txt
- ARC data CC BY 4.0; OSM-derived rows ODbL: https://www.arcreport.ai/data
- Agent Outcome Report spec (tell ARC what happened when your agent tried to reach checkout): https://www.arcreport.ai/docs/outcome-report

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

Every result is dated and labelled by source: agent-reported, ARC-tested up to payment, or a verified order. A missing result never counts against a store.

## License

Repo contents: MIT. ARC data: CC-BY-4.0.

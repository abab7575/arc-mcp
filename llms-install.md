# Installing the ARC MCP server

ARC (https://www.arcreport.ai) is a **hosted remote MCP server**. Nothing to install, build, or run locally, and no API key is needed for the free tools.

- Endpoint: `https://www.arcreport.ai/api/mcp`
- Transport: **Streamable HTTP** (JSON-RPC over HTTP POST; MCP protocol version `2025-03-26`)
- Auth: none for `check_store`, `find_stores`, `report_checkout`, and the other free read tools. Account tools (scans, monitors) need ARC sign-in.

## Cline

1. In VS Code, open the Cline panel → **MCP Servers** → **Configure** (or **Remote Servers**), which opens `cline_mcp_settings.json`.
2. Add ARC under `mcpServers`:

```json
{
  "mcpServers": {
    "arc": {
      "type": "streamableHttp",
      "url": "https://www.arcreport.ai/api/mcp",
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

3. Save. ARC should show as connected with its tools listed.

## Any other MCP client

Use the remote/HTTP server option with this URL. Common config shape:

```json
{
  "mcpServers": {
    "arc": {
      "type": "http",
      "url": "https://www.arcreport.ai/api/mcp"
    }
  }
}
```

(Some clients call the type `streamable-http` or `streamableHttp`; Claude Code: `claude mcp add --transport http arc https://www.arcreport.ai/api/mcp`.)

If your client only supports local stdio servers, bridge with `mcp-remote`:

```json
{
  "mcpServers": {
    "arc": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://www.arcreport.ai/api/mcp"]
    }
  }
}
```

## Quick test

Ask your agent: *"Use ARC to check whether agents can check out on allbirds.com."* It should call `check_store` with `{"domain": "allbirds.com"}` and return dated checkout evidence.

Or from a shell:

```bash
curl -s -X POST https://www.arcreport.ai/api/mcp \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json, text/event-stream' \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/list"}'
```

## Tools

No sign-in needed:
- `check_store`: saved, dated checkout evidence for one store domain
- `find_stores`: stores where an agent reached checkout, by platform, protocol (ucp, webmcp, mcp), or category
- `report_checkout`: report how far an agent got at a store (no personal data stored)
- `get_store_hub`: free ARC store card by host
- `google_checkout_readiness`: saved Google agentic checkout readiness checks
- `product_data_check`: saved product data and return/shipping policy check
- `can_agent_checkout`: free live HTTP check of whether an agent can buy from a store

Signed-in (ARC account): `check_agent_readiness`, `get_readiness_result`, `list_readiness_checks`, `ask_agents_about_brand`, `get_fixes`, `bulk_create_monitors`, `list_monitors`, `monitor_summary`, `update_monitor`, `pause_monitor`, `resume_monitor`, `run_monitor_now`.

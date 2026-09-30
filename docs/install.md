# Install LeaseIQ Pro MCP

## Prerequisite

Create a Free Trial key (50 calls/month, no credit card) at:

https://mcpize.com/mcp/leaseiq-pro

Or use x402 USDC pay-per-call with no key (see [mcpize.yaml](../mcpize.yaml)).

## Cursor

Add to `~/.cursor/mcp.json` (or project `.cursor/mcp.json`):

```json
{
  "mcpServers": {
    "leaseiq-pro": {
      "url": "https://leaseiq-pro.mcpize.run/mcp",
      "headers": {
        "Authorization": "Bearer YOUR_MCPIZE_API_KEY"
      }
    }
  }
}
```

Restart Cursor. Confirm tools appear: `calculate_lease`, `solve_lease_structure`, `generate_amortization_schedule`, `calculate_auto_lease`, `calculate_auto_loan`.

## Claude Desktop

Claude Desktop expects a local stdio server. Bridge the remote HTTP endpoint with `mcp-remote`:

```json
{
  "mcpServers": {
    "leaseiq-pro": {
      "command": "npx",
      "args": [
        "-y",
        "mcp-remote",
        "https://leaseiq-pro.mcpize.run/mcp",
        "--header",
        "Authorization: Bearer YOUR_MCPIZE_API_KEY"
      ]
    }
  }
}
```

Config path (macOS): `~/Library/Application Support/Claude/claude_desktop_config.json`

## Claude Code / other HTTP MCP clients

Point the client at:

```
https://leaseiq-pro.mcpize.run/mcp
```

Header:

```
Authorization: Bearer YOUR_MCPIZE_API_KEY
```

Transport: Streamable HTTP (`/mcp`).

## Smoke test

Ask the agent:

> Using LeaseIQ Pro, calculate an auto loan on a $32,000 vehicle, 60 months, 6.9% APR, $2,000 down.

Expected tool: `calculate_auto_loan`.

## Without an API key (x402)

```bash
curl -sS -X POST 'https://leaseiq-pro.mcpize.run/mcp' \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json, text/event-stream' \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/call","params":{"name":"calculate_auto_loan","arguments":{"vehiclePrice":30000,"term":60,"rate":5.9}}}'
```

You should receive **HTTP 402** with an x402 payment challenge (USDC on Base). Clients that implement x402 can settle and retry.

## Troubleshooting

| Symptom | Fix |
| --- | --- |
| Tools missing | Restart client; verify JSON config path |
| 401 / auth errors | Regenerate key on MCPize; check `Bearer ` prefix |
| 402 Payment Required | Expected without key; use Free Trial key or x402 payment |
| Wrong math domain | Use `calculate_lease` for equipment, `calculate_auto_lease` for cars |

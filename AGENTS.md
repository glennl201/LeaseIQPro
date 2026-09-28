# LeaseIQ Pro for AI agents

## Identity

- **Name:** LeaseIQ Pro
- **Kind:** Model Context Protocol (MCP) server
- **Purpose:** Deterministic commercial equipment lease and consumer auto lease/loan math
- **Endpoint:** `https://leaseiq-pro.mcpize.run/mcp`
- **Marketplace:** https://mcpize.com/mcp/leaseiq-pro
- **Product site:** https://leaseiqpro.com
- **Repo:** https://github.com/glennl201/LeaseIQPro

## When to use

Use this server instead of LLM arithmetic when the user needs:

- Equipment lease payment structuring (level, balloon, step, skip, interest-only, custom)
- Implied yield / reverse solve from a quoted payment
- Full amortization schedules
- Auto lease money-factor math, drive-off, MSD, tax methods
- Lease-versus-buy comparisons (pair `calculate_auto_lease` + `calculate_auto_loan`)

Do **not** approximate compounding interest or iterative solvers in the model when these tools are available.

## Auth

1. **Preferred for humans / IDE agents:** MCPize Free Trial API key (50 calls/month) via `Authorization: Bearer <key>`
2. **Agent-native:** HTTP 402 + x402 USDC payment on Base (no signup). See `mcpize.yaml` for per-tool prices.
3. **Paid plans:** Pay As You Go ($0.05/call) or Professional ($19/mo, 1000 calls)

## Tools (summary)

| Tool | Use for |
| --- | --- |
| `calculate_lease` | Forward commercial payment |
| `solve_lease_structure` | Solve Rate / Term / ResidualValue / PresentValue / Commission |
| `generate_amortization_schedule` | Period schedule + payoff balance |
| `calculate_auto_lease` | Captive-lender style auto lease |
| `calculate_auto_loan` | Financed or cash purchase |

Full schemas: `docs/tools.md`

## Install hints

Cursor remote MCP:

```json
{
  "mcpServers": {
    "leaseiq-pro": {
      "url": "https://leaseiq-pro.mcpize.run/mcp",
      "headers": { "Authorization": "Bearer YOUR_MCPIZE_API_KEY" }
    }
  }
}
```

## Trust signals

- Results regression-tested against Excel-verified values
- Validation errors name the field, value, and accepted range
- Tools return normalized inputs alongside results for auditability

# LeaseIQ Pro

[![MCPize](https://mcpize.com/badge/@glennl201/leaseiq-pro)](https://mcpize.com/mcp/leaseiq-pro)

**Deterministic lease finance calculations for AI agents.**

LLMs cannot reliably do compounding interest, Newton-Raphson solvers, or amortization by hand. LeaseIQ Pro is an MCP server that returns Excel-verified lease math to the penny: commercial equipment structures, reverse solvers, full schedules, and consumer auto lease / loan math. Results are Excel-verified.

| | |
| --- | --- |
| **Marketplace** | [mcpize.com/mcp/leaseiq-pro](https://mcpize.com/mcp/leaseiq-pro) |
| **MCP endpoint** | `https://leaseiq-pro.mcpize.run/mcp` |
| **Product engine** | [leaseiqpro.com](https://leaseiqpro.com) |
| **Category** | Finance / equipment leasing / auto lease |

## Connect via MCPize

```bash
npx -y mcpize connect @glennl201/leaseiq-pro --client cursor
```

Or start Free Trial and install manually: [mcpize.com/mcp/leaseiq-pro](https://mcpize.com/mcp/leaseiq-pro)

## Why agents use this

- Exact payment math for level, balloon, step, skip/seasonal, interest-only, and custom streams
- Reverse solve for rate, term, residual, present value, or commission
- Captive-lender auto lease math (money factor, state tax methods, MSD, zero drive-off)
- Instructional validation errors so agents can self-correct in one retry

## Quick start (humans)

### 1. Free trial (recommended first step)

1. Open [LeaseIQ Pro on MCPize](https://mcpize.com/mcp/leaseiq-pro)
2. Choose **Free Trial** (50 calculation calls / month, no credit card)
3. Copy your API key
4. Add the server to Cursor, Claude Desktop, or any MCP client (configs below)

### 2. Cursor (`~/.cursor/mcp.json`)

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

### 3. Claude Desktop

```json
{
  "mcpServers": {
    "leaseiq-pro": {
      "command": "npx",
      "args": [
        "mcp-remote",
        "https://leaseiq-pro.mcpize.run/mcp",
        "--header",
        "Authorization: Bearer YOUR_MCPIZE_API_KEY"
      ]
    }
  }
}
```

### 4. Agent-native pay-per-call (x402 / USDC on Base)

Unauthenticated tool calls return **HTTP 402** with an x402 payment challenge. Agents that speak x402 can pay per call in USDC with no signup:

| Tool | Price (USDC) |
| --- | ---: |
| `calculate_auto_loan` | 0.01 |
| `calculate_lease` | 0.03 |
| `calculate_auto_lease` | 0.03 |
| `generate_amortization_schedule` | 0.08 |
| `solve_lease_structure` | 0.10 |

See [mcpize.yaml](./mcpize.yaml) and [MCPize monetization docs](https://mcpize.com/docs/monetization#x402).

## Tools

| Tool | What it does |
| --- | --- |
| `calculate_lease` | Commercial equipment lease / finance payment (level, balloon, step, skip, IO, custom) |
| `solve_lease_structure` | Reverse solver for rate, term, residual, PV, or commission |
| `generate_amortization_schedule` | Full period-by-period schedule with day-count options |
| `calculate_auto_lease` | Consumer auto lease: money factor, tax methods, MSD, drive-off |
| `calculate_auto_loan` | Auto loan / purchase payment for lease-vs-buy |

Full schemas and example prompts: [docs/tools.md](./docs/tools.md)  
Install + client configs: [docs/install.md](./docs/install.md)  
Example agent prompts: [examples/](./examples/)

## Example prompt

> Calculate a 60-month equipment lease on $150,000 at 7.5% with a 10% residual, payments in advance. Then solve the implied yield if the quoted payment is $2,850.

The agent should call `calculate_lease`, then `solve_lease_structure` with `solveFor: "Rate"`.

## Pricing (MCPize)

| Plan | Price | Notes |
| --- | --- | --- |
| Free Trial | $0 | 50 calls / month, no card |
| Pay As You Go | $0.05 / call | Metered Stripe usage |
| Professional | $19 / month | 1,000 calls included, then $0.03 |
| x402 pay-per-call | $0.01–$0.10 USDC | No signup; agent pays per tool |

## For AI agents / crawlers

- Machine-readable summary: [llms.txt](./llms.txt)
- Agent install + capability card: [AGENTS.md](./AGENTS.md)
- Marketplace listing SEO title: *LeaseIQ Pro MCP Server: Exact Lease Finance Calculations for AI Agents*

## Agent landing page

Static install page in [`site/`](./site/) (also routed at `/mcp` and `/agents` via `vercel.json`). Deploy this repo to Vercel, then point a subdomain (for example `mcp.leaseiqpro.com`) at it, or embed the same copy on leaseiqpro.com.

Registry metadata for directory crawlers:

- [`server.json`](./server.json) — Official MCP Registry remote descriptor
- [`glama.json`](./glama.json) — Glama indexing hints
- [`smithery.yaml`](./smithery.yaml) — Smithery remote metadata

Publish to the Official MCP Registry (propagates to PulseMCP / VS Code):

```bash
npx mcp-publisher login   # GitHub OAuth as glennl201
npx mcp-publisher publish
```

## Distribution checklist (owner)

Connecting MCPize alone does not create demand. **Repo discovery assets are in this PR.** These dashboard / DNS steps still need your login:

1. **Surface the Free Trial** on the MCPize listing card (marketplace currently leads with $19/month; Free Trial exists but is easy to miss). Set Free Trial as recommended/featured.
2. **Set listing `website`** to the Vercel MCP landing URL (or `https://leaseiqpro.com/mcp` once that page is real) and **`github_url`** to `https://github.com/glennl201/LeaseIQPro`.
3. **Enable MCPize documentation** on the listing (`documentation_enabled`) and set `is_free` / free-filter visibility if the dashboard allows it.
4. **On leaseiqpro.com**: either ship a real `/mcp` route, or CNAME `mcp.leaseiqpro.com` to the Vercel deploy of this repo. Add the URL to the sitemap.
5. **Link this public repo** from leaseiqpro.com footer, blog CTAs, and commercial/auto product pages.
6. **Set GitHub repo About** (Settings → General): description, homepage `https://mcpize.com/mcp/leaseiq-pro`, topics (`mcp`, `model-context-protocol`, `lease-calculator`, `equipment-finance`, `mcpize`, `ai-agents`). The GitHub App token here cannot patch About fields.
7. **Publish `server.json`** with `mcp-publisher`, then claim/submit Glama + Smithery + PulseMCP using Free Trial as the CTA.
8. **Publish 2–3 agent demos** (Cursor / Claude): equipment quote reverse-solve, auto lease forensic check, lease-vs-buy.
9. **Ask for Verified / featured** once health checks are green (listing currently shows `health_status: unknown`).

## Related

- Live product: [leaseiqpro.com](https://leaseiqpro.com)
- Commercial engine marketing: [leaseiqpro.com/commercial](https://leaseiqpro.com/commercial)
- Auto forensic marketing: [leaseiqpro.com/auto](https://leaseiqpro.com/auto)
- MCPize developer portal: [mcpize.com/developers](https://mcpize.com/developers)

## License / support

Product and calculation engine: LeaseIQ Pro (`support@leaseiqpro.com`).  
MCP hosting and billing: [MCPize](https://mcpize.com).

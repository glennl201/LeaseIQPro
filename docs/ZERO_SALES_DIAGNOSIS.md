# Why LeaseIQ Pro has had ~0 MCP sales (diagnosis)

Live check date: 2026-09-28. Listing: https://mcpize.com/mcp/leaseiq-pro  
Endpoint: https://leaseiq-pro.mcpize.run/mcp (healthy; tools/list OK)

## Verdict

The MCP is **live and monetized**, but **almost nobody can discover or try it**. Connecting to MCPize publishes a listing; it does not create demand. Zero subscribers after ~2.5 months matches a discovery + funnel problem, not a broken calculator.

## Evidence from the live listing

| Signal | Observed |
| --- | --- |
| `subscribers_count` | **0** |
| `reviews_count` / rating | **0** / null |
| `featured` | **false** |
| `is_free` | **false** (despite an active Free Trial plan) |
| `website` / `github_url` | **null** / **null** |
| `documentation_enabled` | **false** |
| `health_status` | **unknown** (never checked) |
| `pricing` (server field) | **null** in DB, but **x402 is live** on the gateway |
| Stripe Connect | **active** (`acct_…`) |
| Listing created | 2026-07-05; last update same day |
| This GitHub repo | Public but empty until this PR (no About topics/homepage writable by CI) |
| Deploy source `LeaseFinanceCalc-App` | **Not publicly resolvable** (private or removed) |
| `leaseiqpro.com/mcp` | Returns marketing homepage SPA — **no agent install page** |

## Root causes (ordered by impact)

### 1. No inbound discovery surface

Organic MCP buyers and agents find servers via GitHub, product sites, directories, and marketplace search. LeaseIQ had:

- An empty public `LeaseIQPro` repo
- No website/GitHub links on the MCPize listing
- No dedicated agent install URL on leaseiqpro.com
- No directory submissions tied to a Free Trial CTA

Result: the listing only works if someone already knows the slug.

### 2. Funnel friction for the actual buyer (agents)

Unauthenticated tool calls correctly return **HTTP 402** (x402 USDC). That is great for agent-native billing, but:

- Most IDE agents today speak **API keys**, not x402
- Free Trial exists (50 calls, no card) but the marketplace card leads with **$19/month**
- `is_free: false` keeps the server out of free/trial filters that drive first installs

Top MCPize servers almost always expose a **Free** plan as the first click. LeaseIQ has the plan; it is not the lead offer.

### 3. Niche demand + crowded marketplace

MCPize has 1000+ servers. Even category leaders sit around tens of subscribers, not thousands. Lease finance for agents is a narrow job-to-be-done unless you push it into broker/lessor/fintech agent workflows yourself.

### 4. Product site sells the SaaS app, not the MCP

leaseiqpro.com is optimized for commercial originators and auto forensic users. Robots.txt and sitemap promote `/commercial`, `/auto`, `/blog` — not MCP install. AI crawlers are allowed, but they only see the human SaaS story.

## Status: what is fixed vs still blocked

### Done in this repo / PR

- Rich README with Free Trial → install → x402 path
- `mcpize.yaml`, `AGENTS.md`, `llms.txt`, install/tool docs, example prompts
- Official Registry `server.json`, `glama.json`, `smithery.yaml`
- Static MCP landing page (`site/`) + `vercel.json` routes for `/mcp` and `/agents`

### Attempted, blocked by access

| Action | Result |
| --- | --- |
| PATCH MCPize `website` / `github_url` / `documentation_enabled` / `is_free` | Anon API returns no rows (RLS). Needs your MCPize dashboard login. |
| PATCH Free Trial plan to `featured` / `is_recommended` | Same RLS block. |
| GitHub About description / homepage / topics | GitHub App token returns **403**. Needs your GitHub Settings UI. |
| Edit leaseiqpro.com `/mcp` | Product app is not in this repo; FinanceTeam Vercel has no LeaseIQ project. |
| Directory submit forms (PulseMCP) | Bot-blocked / needs human OAuth. |

### What you still must click

1. MCPize listing: set **website** + **github_url**, enable **documentation**, make **Free Trial** the lead/recommended plan, request health check / Verified.
2. Deploy or connect this repo’s `site/` on Vercel; optionally CNAME `mcp.leaseiqpro.com`. Or ship `/mcp` inside the main product app.
3. GitHub About on this repo: description, homepage, topics.
4. `npx mcp-publisher login && npx mcp-publisher publish`, then claim Glama / Smithery.
5. Publish 2–3 short Cursor/Claude demos.

Until the MCPize listing fields and a real public `/mcp` URL are live, marketplace SEO alone will not produce transactions.

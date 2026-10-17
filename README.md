# SEO·AEO·GEO MCP server

Give your coding agent an **SEO, AEO and GEO audit it can act on**. One remote MCP server audits any public page the way Google and the AI search crawlers see it, and returns a **fix pack**: the fixes in priority order, where each one lives in your codebase, and a command to verify it. Your agent applies the code fixes on a branch, lists the settings you need to change, and re-runs the audit to prove they worked.

- **Endpoint:** `https://seoaeogeo.app/mcp` (Streamable HTTP)
- **Free, no account, no API key**: same free audit as [seoaeogeo.app](https://seoaeogeo.app)
- **Website:** [seoaeogeo.app](https://seoaeogeo.app) · [setup page](https://seoaeogeo.app/agents) · [methodology](https://seoaeogeo.app/methodology)

## Add it

**Claude Code**

```bash
claude mcp add --transport http seoaeogeo https://seoaeogeo.app/mcp
```

**Cursor** (`.cursor/mcp.json`) · **VS Code** (`.vscode/mcp.json`)

```json
{
  "mcpServers": {
    "seoaeogeo": { "url": "https://seoaeogeo.app/mcp" }
  }
}
```

**Codex** (`~/.codex/config.toml`)

```toml
[mcp_servers.seoaeogeo]
url = "https://seoaeogeo.app/mcp"
```

**Claude Code skill** (audit, fix on a branch, re-check)

```bash
npx skills add aispiringcoder4302/seo-aeo-geo-mcp
```

Or clone this repo: `.mcp.json` at the root connects any MCP client that reads it.

Then ask:

> Audit https://your-site.com with seoaeogeo, fix what you can in this repo on a new branch, and tell me which settings I need to change.

## Tools

| Tool | What it does |
|---|---|
| `audit_url` | Audits one public page in 10–30 s. Reads robots.txt per crawler, requests the page as **OAI-SearchBot, Claude-SearchBot and PerplexityBot** and compares each with a browser request (CDN and firewall blocks), measures content without JavaScript, and scores extractability and trust signals. Returns the score, every check's result, the fix pack and structured data. |
| `get_report` | Returns an audit from the last 24 hours by id, so the agent can re-read the fixes without re-running the crawl. |

## What the audit checks

58 checks across four groups, each graded by the strength of the evidence behind it ([methodology](https://seoaeogeo.app/methodology)):

- **Retrieval**: page fetch, indexability, robots.txt rules per AI crawler, CDN and firewall blocks, raw-HTML visibility, snippet controls, HTTPS, soft 404, canonical, load time
- **Extractability**: answer-first writing, semantic HTML, headings, specificity, passage structure
- **Authority**: authorship, dates and freshness, About and Contact pages, entity signals, sources
- **Hygiene**: sitemap, internal links, metadata, alt text, Open Graph

`llms.txt` and schema types are reported but score zero: no controlled study shows they change which pages AI answers cite, and Google says it needs neither.

## Example output (trimmed)

```text
SEO·AEO·GEO audit of https://example.com/: 64/100 (grade D).
Retrieval 64/67 · Extractability 7/26 · Authority 6/18 · Hygiene 2/13
15 issues to fix. Full report: https://seoaeogeo.app/report/…

# Fix pack for example.com
## Fixes in the codebase (3)
### 1. Answer first: fail (5 pts)
**Found:** …   **Change:** …   **Where:** …   **Verify:** …
## Needs a human: settings outside the codebase
…
```

## Free vs paid

The MCP server returns the same free preview as the website: the score, every check's result and the full fix for the top issues. The remaining fixes are in the full report, unlocked for one site for **$9 / ₹499 once** (no subscription), or for every site with **Pro**. Audits are limited per connection per hour.

## Claude Code skill

`skills/seo-aeo-geo-audit/SKILL.md` teaches Claude Code the audit → fix → verify loop. Copy the folder into `~/.claude/skills/` (or your project's `.claude/skills/`).

## Privacy

The audit fetches only the public URL you give it. Reports are kept for 24 hours. No account is needed for the free audit. See the [privacy policy](https://seoaeogeo.app/privacy).

## License

MIT for the files in this repository. The hosted service is governed by its [terms](https://seoaeogeo.app/terms).

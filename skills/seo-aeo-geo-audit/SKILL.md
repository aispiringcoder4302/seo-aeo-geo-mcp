---
name: seo-aeo-geo-audit
description: Audit a web page for Google, AI Overviews, ChatGPT, Claude and Perplexity visibility with the seoaeogeo MCP server, apply the code fixes on a branch, and verify them. Use when the user asks for an SEO audit, AEO or GEO audit, AI search visibility, "why doesn't ChatGPT mention my site", AI crawler access (GPTBot, OAI-SearchBot, ClaudeBot, PerplexityBot), Cloudflare blocking AI bots, or fixing SEO issues in a site's code.
license: MIT
---

# SEO·AEO·GEO audit → fix → verify

Requires the `seoaeogeo` MCP server: `claude mcp add --transport http seoaeogeo https://seoaeogeo.app/mcp`

## 1. Audit

Call `audit_url` with the public URL of the page the user cares about (homepage, main product or service page, or a key article). It must be publicly reachable; for unreleased changes use a public preview deployment URL.

Read the result: the score, which issues are **blockers** (they cap the score at 39 and usually mean AI crawlers can't read the page), and the fix pack.

## 2. Fix (code only, on a branch)

- Create a branch. Work through "Fixes in the codebase" in order.
- Use each fix's **Where** hint to find the template, layout, head metadata, robots.txt generator or rendering config.
- Make the smallest change that fixes the issue. Don't rewrite unrelated content.
- Never invent facts, author names, dates or statistics. Leave a clearly marked TODO and list it for the user.
- Don't touch CDN, firewall, DNS or hosting settings: they're listed under "Needs a human". Tell the user exactly what to change and where.

## 3. Verify

- Run each fix's **Verify** command against a preview deployment.
- Call `audit_url` again on the preview or deployed URL. Report the score change and which issues now pass.
- Don't deploy to production yourself. Open a pull request.

## Facts to keep straight

- `GPTBot` collects training data; `OAI-SearchBot` decides ChatGPT search. Blocking GPTBot doesn't remove a site from ChatGPT search.
- `ClaudeBot` is training; `Claude-SearchBot` indexes for Claude search; `Claude-User` fetches when a user asks.
- Google AI Overviews and AI Mode use normal Googlebot and need nothing beyond Search eligibility. `Google-Extended` doesn't affect them.
- `llms.txt` and schema don't change which pages AI answers cite in any controlled test; don't present them as ranking fixes.
- Many AI crawlers don't run JavaScript: the content must be in the server's HTML.

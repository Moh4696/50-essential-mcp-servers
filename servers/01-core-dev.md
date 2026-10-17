[← back to README](../README.md)

# core dev — the universal core

install these first. these five work everywhere and earn their slot on almost any machine.

## 1. github

repos, pull requests, issues, code search across your whole org — without leaving the terminal. pair it with sentry and your agent can read a production error and open the fixing PR in one move. the official server replaced all the community forks in 2026; don't use anything else.

- **install:** `claude mcp add --transport http github https://api.githubcopilot.com/mcp/ -H "Authorization: Bearer YOUR_GITHUB_PAT"` (or complete the oauth flow via `/mcp` after adding)
- **link:** https://github.com/github/github-mcp-server
- **gotcha:** scope the token. read-only unless the agent genuinely needs to push. the old `@modelcontextprotocol/server-github` npm package is no longer supported — use the hosted server above.

## 2. context7

the one that stops your agent hallucinating package APIs. instead of generating code against stale training data, it pulls current, version-pinned docs at query time. essential for fast-moving frameworks and cloud sdks.

- **install:** `claude mcp add --transport http context7 https://mcp.context7.com/mcp`
- **codex:** `codex mcp add context7 -- npx -y @upstash/context7-mcp`
- **link:** https://context7.com
- **gotcha:** free, with an optional api key for higher rate limits.

## 3. playwright

a real browser the agent can drive — open pages, click, fill forms, screenshot, run smoke tests you describe in plain english. microsoft maintains it.

- **install:** `claude mcp add playwright npx @playwright/mcp@latest`
- **link:** https://github.com/microsoft/playwright-mcp
- **gotcha:** great for exploratory testing, wrong tool for release-critical regression — you want deterministic scripts there. on windows, claude code defaults to git bash; switch it to cmd or commands fail.

## 4. filesystem

scoped local file access beyond the agent's working directory. stdio, local, fast.

- **install:** `claude mcp add filesystem npx @modelcontextprotocol/server-filesystem ~/projects` (pass the allowed paths as args)
- **link:** https://github.com/modelcontextprotocol/servers/tree/main/src/filesystem
- **gotcha:** only expose the directories you want reachable. this is a broad permission, treat it like one. (this is one of the handful of servers anthropic still actively maintains in the main reference repo.)

## 5. brave search

fast web search inside the agent so it can ground answers without you tab-switching.

- **install:** `claude mcp add brave-search npx @brave/brave-search-mcp-server` (brave api key in env)
- **link:** https://github.com/brave/brave-search-mcp-server
- **gotcha:** needs a free key. keep it in user-scope settings, never commit it. note: the old `@modelcontextprotocol/server-brave-search` reference server was archived — brave maintains its own now.

## 51. firecrawl

reads the page behind a search result. scrape any url into clean markdown or structured json (js-rendered pages included), search the web (optionally with page content), and with a key crawl a site or map its urls. official, from the firecrawl team.

- **install:** `claude mcp add --transport http firecrawl https://mcp.firecrawl.dev/v2/mcp` (no account needed to start)
- **codex:** `codex mcp add firecrawl --url https://mcp.firecrawl.dev/v2/mcp`
- **link:** https://github.com/firecrawl/firecrawl-mcp-server
- **gotcha:** the keyless endpoint is rate-limited and only exposes scrape, search and parse. crawl, map and agent need a firecrawl api key (an `Authorization: Bearer` header on the same url). crawls spend credits per page, so set a limit.

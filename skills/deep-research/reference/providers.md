# Search Provider Guide

## Provider Priority

Use search providers in priority order: **free first, paid last**.

| Priority | Provider | Cost | Setup | Best For |
|----------|----------|------|-------|----------|
| 1 | WebSearch | FREE | None | General search, always available |
| 2 | SearXNG | FREE | Docker | Comprehensive search, no limits |
| 3 | Exa MCP | FREE tier | MCP config | Semantic/neural search |
| 4 | search-cli | PAID | API keys | Multi-provider (fallback only) |

---

## Option A: WebSearch (PRIMARY)

**Cost:** FREE
**Setup:** None (built into agent)
**Availability:** Always

### Usage

```
WebSearch(query="your search query")
```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `query` | Yes | Search query string |
| `allowed_domains` | No | Restrict results to specific domains |
| `blocked_domains` | No | Exclude specific domains from results |

### Best Practices

- Use for all general searches
- Always available - no configuration needed
- Use domain filtering for academic/official sources:
  - `allowed_domains=["arxiv.org", "scholar.google.com"]` for papers
  - `allowed_domains=["docs.example.com"]` for documentation

---

## Option B: SearXNG (FREE LOCAL)

**Cost:** FREE
**Setup:** Docker container (optional)
**Availability:** When running locally

### Setup

```bash
# Clone and configure
git clone https://github.com/vakovalskii/searxng-docker-tavily-adapter
cd searxng-docker-tavily-adapter
cp config.example.yaml config.yaml

# Generate secret key
python -c "import secrets; print(secrets.token_urlsafe(32))"
# Add to config.yaml

# Start
docker compose up -d
```

### Health Check

```bash
curl -s http://localhost:8000/health
```

### Usage

```bash
curl -X POST "http://localhost:8000/search" \
  -H "Content-Type: application/json" \
  -d '{"query": "your query", "max_results": 10}'
```

### Parameters

| Parameter | Required | Default | Description |
|-----------|----------|---------|-------------|
| `query` | Yes | - | Search query |
| `max_results` | No | 10 | Number of results (1-20) |
| `include_raw_content` | No | false | Include full page text |

### Advantages

- No API keys required
- No rate limits
- Privacy-focused (local)
- Tavily-compatible API

### Limitations

- Requires Docker
- Content limited to ~2500 chars per page
- No JavaScript rendering (use tavily-extract for JS pages if available)
- No built-in research synthesis

---

## Option C: Exa MCP (FREE TIER)

**Cost:** FREE tier available
**Setup:** MCP configuration
**Availability:** When configured

### Usage

If Exa MCP is configured in your agent environment:

```
mcp__Exa__exa_search(query="quantum computing", type="neural", num_results=10)
```

### Parameters

| Parameter | Required | Default | Description |
|-----------|----------|---------|-------------|
| `query` | Yes | - | Search query |
| `type` | No | "auto" | "auto", "neural", or "keyword" |
| `num_results` | No | 10 | Number of results |
| `start_published_date` | No | - | Filter by publication date |
| `include_domains` | No | - | Restrict to specific domains |

### Advantages

- Semantic/neural search (understands meaning)
- Good for conceptual queries
- Free tier available

### Limitations

- Requires MCP configuration
- Free tier has limits

---

## Option D: search-cli (PAID - FALLBACK ONLY)

**Cost:** PAID (requires API keys)
**Setup:** Homebrew + API keys
**Availability:** When configured

**Only use if Options A-C are unavailable.**

### Setup

```bash
brew tap 199-biotechnologies/tap && brew install search-cli
search config set keys.brave YOUR_KEY
# or keys.serper, keys.exa, etc.
```

### Usage

```bash
search "query" --json -c 15
search "query" -m academic --json
```

### Parameters

| Flag | Description |
|------|-------------|
| `--json` | Output as JSON |
| `-c N` | Number of results |
| `-m MODE` | Search mode: general, news, academic, etc. |

### Providers Supported

- Brave (API key required)
- Serper (API key required)
- Exa (API key required)
- Jina (API key required)
- Firecrawl (API key required)

---

## Detection Logic

When starting research, check provider availability:

```
1. WebSearch: Always available (built-in)
2. SearXNG: Check http://localhost:8000/health
3. Exa MCP: Check if mcp__Exa__exa_search tool exists
4. search-cli: Check if 'search' command available
```

Use the first available provider from the priority list.

---

## Fallback Strategy

If primary provider fails:

1. **WebSearch fails** → Unlikely (built-in), check network
2. **SearXNG not running** → Use WebSearch, suggest user start Docker
3. **Exa MCP not configured** → Use WebSearch or SearXNG
4. **search-cli not installed** → Good (free options preferred)

**Never fail due to missing paid services.** Always have WebSearch as fallback.

---

## Option E: Browser Automation (FALLBACK)

When all API-based search and extraction fail, use browser automation.

**Cost:** FREE (tools must be installed)
**Setup:** Varies by tool
**Availability:** When browser tools are configured

### Available Browser Tools

| Priority | Tool | Detection |
|----------|------|-----------|
| 1 | playwright-cli skill | Check for skill or `playwright-cli --version` |
| 2 | MCP playwright | Check for `mcp__playwright__*` tools |
| 3 | Other browser MCPs | Check for puppeteer, selenium, etc. |

### When to Use

- JavaScript-rendered content (SPAs, dynamic pages)
- CAPTCHA challenges
- Login-required content (LinkedIn, Twitter)
- Rate-limited APIs
- Direct search engine scraping (DuckDuckGo)

### Usage Pattern

```bash
# Navigate
playwright-cli open "https://example.com"

# Get page structure
playwright-cli snapshot

# Extract content
playwright-cli eval "document.body.innerText"
```

### Headed Mode (Human Handoff)

For CAPTCHA or login:

```bash
# Open visible browser
playwright-cli open "https://example.com" --headed

# Pause for human intervention
# Ask human: "Please complete the [CAPTCHA/login] to continue."

# Resume after human confirms "done"
```

### Best Practices

- Try headless first (faster)
- Switch to headed for CAPTCHA/login
- Use DuckDuckGo for search scraping (no CAPTCHA)
- Save browser state to avoid repeated logins

### Limitations

- Slower than API methods
- May require human intervention
- Different tools have different capabilities

**For detailed patterns, see [browser-scraping.md](./browser-scraping.md).**
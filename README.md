# mcp-marginalia

Marginalia MCP — independent web search over a non-commercial, non-SEO index.

Part of [Pipeworx](https://pipeworx.io) — an MCP gateway connecting AI agents to 1573+ live data sources.

## Tools

| Tool | Description |
|------|-------------|
| `marginalia_search` | Search the INDEPENDENT Marginalia web index — a separate crawler that favours text-heavy, ad-light, non-commercial pages: personal sites, technical writing, documentation, forums, academic and hobbyist pages. Use it when mainstream search results are dominated by SEO content, listicles, and marketing pages, or when you want a second opinion from an index that is NOT Google/Bing-derived (unlike serper_search / exa_search / serpapi). Especially good for programming and niche technical topics, obscure or historical subjects, and finding primary/hobbyist sources. Returns url, title, description, a quality score, and how many other results share the domain. Weak for breaking news, e-commerce, and mainstream/celebrity topics — use serper_search for those. Results are CC-BY-NC-SA 4.0 licensed; the licence is returned with every response. |

## Quick Start

Add to your MCP client (Claude Desktop, Cursor, Windsurf, etc.):

```json
{
  "mcpServers": {
    "marginalia": {
      "url": "https://gateway.pipeworx.io/marginalia/mcp"
    }
  }
}
```

### What this endpoint actually serves

`tools/list` at `https://gateway.pipeworx.io/marginalia/mcp` returns the tools in the table
above **plus the shared Pipeworx meta-tools** — `ask_pipeworx`,
`discover_tools`, `search_within`, `remember`/`recall` and the rest of the
gateway-wide set. So the tool count you see is larger than this table: a
single-pack endpoint currently lists roughly 30 shared tools alongside the
pack's own. The connection's `initialize` response states its exact scope, and
is the authoritative answer for a given day.

This is deliberate, not multiplexing by accident. The meta-tools are what let a
scoped connection answer a question this pack does not cover — via
`ask_pipeworx`, which routes across the whole catalog — without you adding a
second MCP server. There is currently no way to mount a pack endpoint without
them; if the extra schemas cost you more context than the routing is worth,
connect to the full gateway once rather than to several pack endpoints.

Or connect to the full Pipeworx gateway to get every pack's tools listed
directly, instead of just this one's:

```json
{
  "mcpServers": {
    "pipeworx": {
      "url": "https://gateway.pipeworx.io/mcp"
    }
  }
}
```

Both URLs reach the same gateway and the same 1573+ data sources. The
only difference is which pack's tools are listed **directly**; `ask_pipeworx`
reaches all of them from either one.

## Standalone (no gateway account)

This package also runs as a local stdio MCP server — no Pipeworx account, no
gateway round-trip:

```json
{
  "mcpServers": {
    "marginalia": {
      "command": "npx",
      "args": ["-y", "@pipeworx/mcp-marginalia"]
    }
  }
}
```

Or run it directly to confirm it starts:

```bash
npx -y @pipeworx/mcp-marginalia
```

It speaks MCP over stdin/stdout and answers `initialize`/`tools/list`/`tools/call`
for **only** this pack's tools — none of the shared meta-tools the gateway
connection above adds. Same source, same tools, no ask_pipeworx routing.

## Using with ask_pipeworx

Instead of calling tools directly, you can ask questions in plain English —
this works on the pack endpoint above as well as on the full gateway:

```
ask_pipeworx({ question: "your question about Marginalia data" })
```

The gateway picks the right tool and fills the arguments automatically.

## More

- [Docs and guides](https://pipeworx.io/docs)
- [pipeworx.io](https://pipeworx.io)

## License

MIT

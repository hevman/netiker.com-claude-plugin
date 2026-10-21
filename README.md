# Netiker for Claude

Claude plugin for analyzing saved reports from your Netiker account: Google Search Console, Bing Webmaster Tools, Ahrefs and other supported sources. Includes a report-analysis skill and an OAuth-protected remote MCP connection.

## Install the plugin

### Claude and Cowork

Download the plugin ZIP from a successful GitHub Actions run and upload it through Claude's plugin interface where available. The downloaded Actions artifact is an outer ZIP: extract it and upload the `netiker-claude-plugin-<version>.zip` inside.

Organizations can distribute the repository as a GitHub-synced marketplace where their Claude plan and administrator settings support it. Availability and interface labels depend on the Claude surface and account settings.

This repository is a distributable plugin and marketplace. It does not automatically publish Netiker in Anthropic's official directory.

### Claude Code

Add the marketplace and install the bundled plugin:

```text
/plugin marketplace add hevman/netiker.com-claude-plugin
/plugin install netiker@netiker
```

Authenticate the plugin's Netiker connection through `/mcp` when prompted. The plugin bundles the connection; no separate manually configured MCP server is needed. Sign into your Netiker account and approve read access. Provider keys and passwords are never part of this package.

## Example requests

- List my Google Search Console and Bing reports.
- Compare these two reports after checking periods, dimensions and completeness.
- Explain why the latest report is stale using synchronization history.

The analysis skill is also available as `/netiker:netiker` in Claude Code. Tool names are scoped by the host, for example `mcp__plugin_netiker_netiker__list_reports`.

## Data access

The endpoint is https://app.netiker.com/mcp, using Streamable HTTP and OAuth discovery, dynamic client registration and S256 PKCE. Access is read-only and subject to the account's Data API quotas.

Tools: `list_reports`, `get_report`, `read_report_rows`, `list_sync_runs`. They read saved data and metadata, never provider credentials. They do not refresh sources, edit reports or modify campaigns. The skill checks dates, units, currencies, pagination and ingestion truncation; it distinguishes Ahrefs estimates from measured GSC/Bing clicks.

You can revoke access from Netiker account settings. Client OAuth tokens are handled by Claude, not embedded in repository files.

## Development and packaging

```sh
node scripts/validate.mjs
claude plugin validate .
claude plugin validate ./plugins/netiker
```

GitHub Actions validates and builds an uploadable plugin ZIP. The ZIP contains `.claude-plugin/plugin.json`, `.mcp.json`, `skills/`, and `assets/` at its root. The repository's marketplace catalog stays outside the plugin ZIP.

No Node.js server, API key, OpenAI manifest or fabricated application ID is required by the package. End-to-end account authorization still requires signing in through Claude; static validation does not prove a successful login on every Claude surface.

## License

The plugin code in this repository is MIT-licensed. This does not license the Netiker backend, Rust rules engine, service, branding, user reports or provider data. Using the hosted service remains subject to Netiker's terms and account limits.

Official documentation:
- https://code.claude.com/docs/en/plugins-reference
- https://code.claude.com/docs/en/plugin-marketplaces
- https://support.claude.com/en/articles/13837440-use-plugins-in-claude

Website: https://netiker.com

Privacy: https://netiker.com/privacy

Terms: https://netiker.com/terms

Support: https://netiker.com/contact

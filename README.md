# Trikster

A conversational trik that manages your installed triks through natural language. Search the TrikHub registry, install new triks, uninstall or upgrade existing ones — all without leaving your agent's conversation.

## How It Works

Trikster is a **conversational trik** with the `trikManagement` capability. When the user asks to find or manage triks, the main agent hands off to Trikster. Trikster has its own LLM and takes over the conversation, using the gateway's registry API to search, install, and uninstall triks.

When Trikster installs or uninstalls a trik, the change takes effect immediately — the consuming agent is rebuilt with the updated tool set (when using `createAgent`).

### Security Model

All tool results use **logSchema + logTemplate** with constrained types:

- Status fields: enums (`installed`, `failed`, `not_found`, etc.)
- Identifiers: format-restricted strings (`"format": "id"`)
- Counts: integers

No free-form text from Trikster's tool executions reaches the main agent's context.

## Tools

| Tool | Description |
|------|-------------|
| `search_triks` | Search the registry by keyword |
| `list_installed_triks` | Show all installed triks |
| `install_trik` | Install a trik by ID (e.g. `@scope/name`) |
| `uninstall_trik` | Remove an installed trik |
| `upgrade_trik` | Upgrade to a newer version |
| `get_trik_info` | Get detailed info from the registry |

## Installation

```bash
trik install @molefas/trikster
```

## Configuration

Requires an Anthropic API key (the trik runs its own LLM):

```json
// .trikhub/secrets.json
{
  "@molefas/trikster": {
    "ANTHROPIC_API_KEY": "sk-ant-..."
  }
}
```

## Usage

```
User: I want to find some new triks
Agent: [hands off to Trikster]

Trikster: What kind of capability are you looking for?
User: Something for hashing
Trikster: I found these:
  1. @molefas/trik-hash — Compute and compare cryptographic hashes
  Want me to install any of these?
User: Yes, install the first one
Trikster: Installed @molefas/trik-hash v1.0.1. You now have computeHash and compareHash tools.

User: /back
Agent: [back in control, now with hash tools available]
```

## License

MIT

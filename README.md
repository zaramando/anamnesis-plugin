# Anamnesis — Claude Code Plugin

Anamnesis is a persistent memory server for AI agents that lets Claude save, search, and recover context across sessions and share it among team developers.

## Prerequisites

- [Claude Code](https://claude.ai/claude-code) installed and authenticated

## Install

**Step 1:** Add the plugin from the marketplace

```
/plugin marketplace add zaramando/anamnesis-plugin
```

**Step 2:** Install the plugin

```
/plugin install anamnesis
```

**Step 3:** Authenticate in your browser

```
/mcp
```

Follow the OAuth prompt — Claude Code will open your system browser. Sign in with your Anamnesis credentials at https://anamnesis.armandozaratem.com. Once authenticated, the token is stored automatically for future sessions.

> Don't have an account? Create one at https://anamnesis.armandozaratem.com

## What happens after install

On every new session or after `/clear`, the SessionStart hook fires automatically. Claude receives the Anamnesis protocol instructions as additional context and calls `awaken()` on its first turn — no user prompt required. From that point forward:

- `awaken()` recovers recent context and suggests a working project.
- `capture()` saves decisions, discoveries, and findings during the conversation.
- `rest()` seals and summarizes the session when you wrap up.

All memory operations run silently in the background unless you explicitly ask about them.

## Token expiry

When your OAuth token expires, Claude Code opens a browser window for re-authorization automatically. This is expected behavior — complete the login and the session continues without any data loss.

## Self-hosting

If you run your own Anamnesis instance, fork this repo and update the single URL in `.mcp.json`:

```json
{
  "mcpServers": {
    "anamnesis": {
      "type": "http",
      "url": "https://YOUR-INSTANCE/mcp"
    }
  }
}
```

Then install from your fork:

```
/plugin install YOUR-GITHUB-USERNAME/anamnesis-plugin
```

### Troubleshooting: transport fallback

If the `http` transport fails to connect against your instance (Claude Code reports a transport error), change `"type": "http"` to `"type": "sse"` in `.mcp.json`. This is only needed for older Anamnesis instances that have not upgraded to streamable HTTP.

## License

MIT — see [LICENSE](LICENSE).

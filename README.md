# SendAPI Skills

[Agent Skills](https://docs.claude.com/en/docs/claude-code/skills) that teach Claude how to send
**WhatsApp, SMS, OTP, and email** through [SendAPI](https://sendapi.co) — with the right channel,
the correct verification flow, and bulk-send guardrails built in.

The Skill is the *know-how*; the [SendAPI MCP server](https://sendapi.co/mcp) is the *tools*. This
plugin installs both — the skill **and** a connection to the hosted MCP server — in one step.

## Install (Claude Code)

```bash
/plugin marketplace add SendAPI-co/sendapi-skills
/plugin install sendapi
```

Then set your API key in the environment so the MCP server can authenticate:

```bash
export SENDAPI_API_KEY=sk_live_xxxx   # from https://sendapi.co/dashboard
```

Now just ask:

> "Send an SMS to +1 555 000 0000 letting them know their order shipped."

## What's included

| Skill | What it teaches |
|-------|-----------------|
| [`sendapi`](skills/sendapi/SKILL.md) | Choosing a channel (SMS / WhatsApp / email / OTP), the required fields for each, the `send_otp` → `check_otp` verify flow, bulk-send confirmation guardrails, and acting on error codes. |

The plugin's [`.mcp.json`](.mcp.json) registers the hosted SendAPI MCP server
(`https://sendapi.co/mcp`), so Claude gets real tools — `send_sms`, `send_whatsapp`, `send_email`,
`send_otp`, `check_otp`, and more — alongside the know-how.

## Manual install (without the plugin)

Copy the skill folder into your skills directory:

```bash
mkdir -p ~/.claude/skills/sendapi
cp -r skills/sendapi/SKILL.md skills/sendapi/references ~/.claude/skills/sendapi/
```

For a single project, place it at `.claude/skills/sendapi/` instead. The skill works on its own
through the REST API (`https://sendapi.co/v1`) when the MCP server isn't connected — set
`SENDAPI_API_KEY` in the environment.

## Requirements

- A SendAPI API key — [free from the dashboard](https://sendapi.co/dashboard).
- Claude Code or Claude Desktop.

## Links

- [Documentation](https://sendapi.co/docs)
- [MCP Server](https://sendapi.co/docs/mcp.html)
- [Agent Skills docs](https://sendapi.co/docs/skills.html)

## License

MIT — see [LICENSE](LICENSE).

# Troubleshooting

## Server fails with "Invalid configuration"

- Confirm `--zulip-config-file` points to an existing file.
- If omitted, verify one of these exists:
  - `./zuliprc`
  - `~/.zuliprc`
  - `~/.config/zulip/zuliprc`
- Or provide env fallback: `ZULIP_EMAIL`, `ZULIP_API_KEY`, `ZULIP_SITE`.

## 401 Unauthorized

- Regenerate API key in Zulip personal settings.
- Ensure the key/email belongs to the same Zulip realm as `site`.
- Re-test by running setup wizard validation:

```bash
uvx --from zulipchat-mcp zulipchat-mcp-setup
```

## Bot identity cannot be selected

- Provide `--zulip-bot-config-file` or bot env credentials.
- Call `server_info` to confirm bot availability.

## Tool not found in client

- You are likely in core mode.
- Start with `--extended-tools` (or `ZULIPCHAT_EXTENDED_TOOLS=1`) for full tool set.

## `request_user_input` or approvals never resolve

- Ensure the message listener is running. It lazy-starts on the first request-wait or event-poll call.
- Confirm the reply happened in the bound session topic, not a different topic or DM.
- Confirm the reply came from the configured owner account.

## Claude hook bridge does nothing

- Make sure you installed the hook entrypoint: `zulipchat-mcp-hook`.
- Confirm Claude Code hooks are invoking the bridge with the same Zulip config files as the MCP server.
- For `SessionEnd` hooks, increase `CLAUDE_CODE_SESSIONEND_HOOKS_TIMEOUT_MS` if you need more than Claude Code’s short default timeout.

## Session messages say "Not authorized"

- Owner policy is per session topic. Only the configured owner email can steer or approve a bound session by default.
- If the bot is in a broader channel, unauthorized users can still mention it normally outside a bound session topic; the restriction applies to the bound control topic.

## Event queue errors

- Re-register using `register_events`.
- Validate `queue_id` and `last_event_id` sequence.

## File download/share failures

- Verify the file identifier is a valid upload path or full URL.
- Ensure the active identity has access to the underlying stream/DM context.

## Setup wizard EOF in non-interactive shells

The wizard is interactive. Run it directly in a terminal (no piped stdin):

```bash
uvx --from zulipchat-mcp zulipchat-mcp-setup
```

## More help

- [Quick Start](user-guide/quick-start.md)
- [Configuration](user-guide/configuration.md)
- [Integration docs](integrations/README.md)
- [Support](../SUPPORT.md)

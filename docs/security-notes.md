# Security Notes

The production source code and operational configuration are private.

This public repository intentionally excludes:

- source code for the production backend, bot, frontend, and runner
- `.env` files
- API keys and tokens
- Discord bot tokens
- Discord webhook URLs
- guild IDs, channel IDs, and user IDs
- raw workflow exports
- private prompts
- deployment details
- machine-specific paths and hostnames

## Secret Boundary

The project uses secret references in configuration and traces. Public documentation may show placeholders such as:

```text
{{secret.OPENAI_API_KEY}}
{{secret.DISCORD_BOT_TOKEN}}
{{secret.DISCORD_ROOM_WEBHOOK}}
```

Actual secret values should never appear in screenshots, logs, traces, prompts, or public docs.

![Secret references](../assets/screenshots/secret-references.png)

The frontend stores and displays secret references, descriptions, and configured/empty status. It does not need to display the actual secret value after configuration. AI tools should only receive placeholders such as `{{secret.GEMINI_API_KEY_TIER1}}`; the backend resolves the real value only at runtime.

This boundary matters because model profiles, Discord bot credentials, and webhook URLs are operational secrets. They can be referenced by configuration, prompt compilation, and workflow traces without exposing the underlying key.

## Screenshot Checklist

Before adding screenshots to this repository:

- hide Discord server/channel names if they are private
- hide usernames if needed
- remove or blur message IDs, channel IDs, and webhook URLs
- avoid showing actual API keys, model keys, or bearer tokens
- avoid showing private prompt content if it reveals sensitive behavior
- prefer staged/demo messages over personal conversation history

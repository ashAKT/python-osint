# Telegram Threat Intelligence Monitor

A Python-based Telegram OSINT helper that discovers relevant public channels, scans recent messages in accessible channels, and flags posts that contain configured client identifiers together with potential data-exposure indicators.

> This is a collection and triage aid, not proof of a breach. Validate every finding with an analyst and trusted sources. Telegram search is limited; this tool does not search every public channel or perform unrestricted global post search.

## Files

- `telegram_monitor.py` — channel discovery, scanning, matching, and reporting.
- `telegram_test.py` — Telegram authentication/connectivity test.
- `config.json` — discovery terms, client watchlist, and scan options.

## Requirements and setup

Python 3.10 or newer is recommended. From the repository root:

```bash
python -m pip install -r requirements.txt
```

Create a local `.env` file in the repository root (the parent of `telegram/`):

```dotenv
TELEGRAM_API_ID=your_numeric_api_id
TELEGRAM_API_HASH=your_api_hash
```

Get API credentials from https://my.telegram.org/apps. Never commit the `.env` file or a Telethon `.session` file. The first authenticated run may ask you to sign in and enter a Telegram login code.

## Configure

Edit `telegram/config.json`:

- `known_channels`: optional channel usernames/references to scan explicitly.
- `discovery_keywords`: terms used to discover relevant channels through Telegram chat search.
- `client_watchlist`: client names and identifiers to match. Replace the example values with approved identifiers.
- `messages_per_channel`: maximum recent messages inspected per channel per run.
- `max_discovered_channels_per_keyword`: cap on newly discovered channels per search term.
- `scan_existing_dialogs`: include channels/supergroups visible in your Telegram dialogs.
- `store_message_text`: whether to store a redacted excerpt of matching messages.
- `store_source_message_link`: whether to include links when a public channel username is available.

Keep confidential client details in a local configuration and do not commit them.

## Run

From the repository root:

```bash
python telegram/telegram_test.py
python telegram/telegram_monitor.py
```

Review the generated CSV/JSON findings and discovered channel metadata under `telegram/`. These reports may contain sensitive operational information; review them before sharing.

## Limitations and safe use

- Channel discovery uses Telegram chat search and is not exhaustive.
- Only the configured number of recent messages in channels the account can access are scanned.
- Global Telegram post search may be restricted or require Telegram Premium.
- Text matching can miss paraphrases, obfuscated names, images, attachments, and messages without configured terms.
- Redaction is best-effort and cannot guarantee removal of all personal data, credentials, tokens, or secrets.
- A keyword match is an unverified lead, not confirmation of compromise.
- Respect Telegram terms, rate limits, access restrictions, privacy, and applicable law. Never use exposed secrets to access systems.

## Git hygiene

The repository `.gitignore` excludes `.env`, Telethon session files, and generated reports. If a credential or session is exposed, revoke/rotate it as applicable.

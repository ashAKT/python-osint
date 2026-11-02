# Python OSINT

A collection of Python tools for open-source intelligence collection and defensive investigation.

## Telegram OSINT Monitor

The `telegram/` directory contains a Telegram channel discovery and monitoring utility. It searches for relevant public channels, scans recent messages in channels accessible to the authenticated Telegram account, and flags possible client-related data-exposure indicators for analyst review.

### Requirements

- Python 3.10 or newer
- Telegram account and API credentials from https://my.telegram.org/apps

Install dependencies from the repository root:

```bash
python -m pip install -r requirements.txt
```

Create a repository-root `.env` file:

```dotenv
TELEGRAM_API_ID=your_numeric_api_id
TELEGRAM_API_HASH=your_api_hash
```

Review `telegram/config.json` and replace the example watchlist with identifiers you are authorized to monitor. Run from the repository root:

```bash
python telegram/telegram_test.py
python telegram/telegram_monitor.py
```

See [telegram/README.md](telegram/README.md) for configuration, outputs, limitations, and security guidance.

## Security

Never commit `.env`, Telethon `.session` files, session journals, real client watchlists, or generated findings. Keep credentials and session files private. Findings are unverified leads and require analyst validation. Respect Telegram's terms, rate limits, privacy, channel access restrictions, and applicable law.

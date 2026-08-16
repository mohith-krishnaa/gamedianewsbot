# Gaming News Telegram Bot

A Python automation bot that scrapes gaming news from **GameRant** and publishes formatted updates to a Telegram channel.

## Live Demo

📢 **Telegram Channel:** https://t.me/GamediaNews_acn

The Telegram channel is the live output of the automation pipeline.

## Pipeline

```text
GameRant
   ↓
Scraper / parser
   ↓
Date + duplicate filtering
   ↓
Image processing
   ↓
Telegram formatter
   ↓
Telegram channel
```

## Features

- Fetches recent gaming news from GameRant
- HTML-formatted Telegram captions
- Optional banner-composited images using Pillow
- Duplicate prevention with local JSON state
- Asia/Kolkata timezone-aware date filtering
- Async Telegram publishing
- Retry handling for transient Telegram failures
- Text-only fallback when image publishing fails
- GitHub Actions / cron-friendly execution

## Requirements

- Python 3.9+
- Telegram Bot Token
- Telegram channel/group ID
- Network access to GameRant and Telegram

## Installation

```bash
git clone https://github.com/mohith-krishnaa/gamedianewsbot.git
cd gamedianewsbot
python -m venv .venv
```

### Windows

```powershell
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

### Linux / macOS

```bash
source .venv/bin/activate
pip install -r requirements.txt
```

## Configuration

The bot reads these environment variables directly:

```text
BOT_TOKEN=<telegram-bot-token>
CHANNEL_ID=<telegram-channel-or-chat-id>
```

The source uses `os.environ.get("BOT_TOKEN")` and `os.environ.get("CHANNEL_ID")`, so the names above are the exact variable names expected by the application.

**Never commit a real Telegram token to Git.**

For GitHub Actions, add the values as repository secrets and expose them to the workflow environment.

## Duplicate handling

Published article identifiers are stored in `posted.json`. This prevents repeated scheduled runs from continuously publishing the same articles.

`posted.json` is local runtime state, not a database. If the runtime filesystem is ephemeral, duplicate history will not survive a fresh environment unless the state is persisted separately.

## Image processing

When an article image is available, the bot downloads the configured banner and composites it over the article image using Pillow. If image processing fails, the bot can fall back to a text-only Telegram message.

## Reliability

Transient Telegram failures are retried up to the configured retry count. The scraper and publisher should still be treated as dependent on upstream website structure, network availability, and Telegram API behavior.

## Scheduling

The repository includes a GitHub Actions workflow and can also be run through a normal cron scheduler.

## Limitations

- GameRant HTML changes can require scraper updates.
- Upstream image URLs can expire or reject requests.
- Telegram imposes API and media limits.
- `posted.json` is not designed for concurrent writers.
- A public scheduled bot should use appropriate GitHub Actions/resource limits.

## Content notice

GameRant articles, images and other third-party content remain subject to their respective rights and terms. This repository does not claim ownership of third-party material.

## License

See the repository license for the applicable terms.

## Author

**Mohith Krishnaa** — [@mohith-krishnaa](https://github.com/mohith-krishnaa)

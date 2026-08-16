# Gaming News Telegram Bot

A Python automation bot that scrapes gaming news from **GameRant** and publishes formatted updates to a Telegram channel.

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
- Asia/Kolkata timezone-aware filtering
- Async Telegram publishing
- Retry handling for transient failures
- Text-only fallback when image publishing fails
- Suitable for cron or GitHub Actions execution

## Requirements

- Python 3.9+
- Telegram Bot Token
- Telegram channel/group ID
- Network access to the source website and Telegram API

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

Configure the Telegram credentials using the mechanism expected by the source code. Keep secrets outside Git.

Typical deployment values are:

```text
BOT_TOKEN=<telegram-bot-token>
CHANNEL_ID=<telegram-channel-or-chat-id>
```

Verify the exact variable names in the current code before running the bot.

## Duplicate handling

The bot stores previously published article information locally so repeated scheduled executions do not continuously republish the same content.

If the runtime uses an ephemeral filesystem, duplicate state may need to be moved to durable storage.

## Reliability

Retry logic handles transient network and Telegram failures. If image generation or delivery fails, the bot can fall back to a text-only publication.

## Scheduling

The application is designed to run as a scheduled job. GitHub Actions or a conventional cron scheduler can invoke it periodically.

## Limitations

- Scraping depends on GameRant's current HTML structure and availability.
- Upstream markup changes can require parser updates.
- Telegram imposes request and media limits.
- Local JSON state is not a database and is not suitable for concurrent writers.

## Content notice

GameRant articles, images and other third-party content remain subject to their respective rights and terms. This repository does not claim ownership of third-party material.

## License

See the repository license for the applicable terms.

## Author

**Mohith Krishnaa** — [@mohith-krishnaa](https://github.com/mohith-krishnaa)

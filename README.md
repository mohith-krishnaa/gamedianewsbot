# Gaming News Telegram Bot

A Python automation pipeline that collects recent gaming news from **GameRant**, filters and formats it, processes images, and publishes updates to Telegram.

**Live output:** https://t.me/GamediaNews_acn

> **Status:** Personal automation / research project. Upstream site structure and Telegram limits can change at any time.

## What it does

```text
GameRant
   ↓
Scrape + parse
   ↓
Date filtering
   ↓
Duplicate protection
   ↓
Image processing
   ↓
Telegram formatting
   ↓
Publish
```

## Features

- Fetches recent gaming news from GameRant
- Filters articles by date and previously published IDs
- HTML-formatted Telegram captions
- Optional banner compositing with Pillow
- Async Telegram publishing
- Retry handling for transient Telegram failures
- Text-only fallback when image processing/publishing fails
- GitHub Actions / cron-friendly execution
- Asia/Kolkata timezone-aware filtering

## Requirements

- Python 3.9+
- Telegram Bot Token
- Telegram channel/group ID
- Network access to GameRant and Telegram

## Quick start

```bash
git clone https://github.com/mohith-krishnaa/gamedianewsbot.git
cd gamedianewsbot
python -m venv .venv
```

Windows:

```powershell
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
python <entrypoint>.py
```

Linux/macOS:

```bash
source .venv/bin/activate
pip install -r requirements.txt
python <entrypoint>.py
```

Use the repository's current Python entrypoint when running locally; the scheduled workflow is the authoritative execution path for the deployed automation.

## Configuration

The application expects:

```text
BOT_TOKEN=<telegram-bot-token>
CHANNEL_ID=<telegram-channel-or-chat-id>
```

Never commit real credentials. For GitHub Actions, store them as repository secrets.

## State and scheduling

`posted.json` contains runtime duplicate-tracking state. It is not a database and should not be treated as durable shared state for concurrent workers.

The repository supports scheduled execution through GitHub Actions as well as normal cron-style execution. The workflow uses the scoped built-in `GITHUB_TOKEN` with `contents: write` permission to persist `posted.json`; it does not require a separate `GH_PAT` secret.

## Reliability

The bot depends on three external systems: GameRant's current HTML structure, network availability, and the Telegram API. Retries handle transient failures, but they cannot compensate for permanent upstream changes.

## Limitations

- GameRant HTML changes may require scraper maintenance.
- Upstream image URLs can expire or reject automated requests.
- Telegram API limits apply to messages and media.
- Local JSON state is unsuitable for concurrent writers or multi-instance deployments.
- Scheduled automation consumes GitHub Actions minutes and should be kept appropriately scoped.
- The workflow still requires `BOT_TOKEN` and `CHANNEL_ID` repository secrets; never place either credential in source control.

## Content notice

GameRant articles, images and other third-party content remain subject to their respective rights and terms. This repository does not claim ownership of third-party material.

## License

See `LICENSE` for the applicable source-code license.

## Author

**Mohith Krishnaa** — [@mohith-krishnaa](https://github.com/mohith-krishnaa)

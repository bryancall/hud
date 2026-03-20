# HUD — Heads-Up Display

A terminal dashboard that shows everything on your radar: GitHub PRs, issues, Jira tickets, and local weather. One glance, no context switching.

![HUD screenshot](https://raw.githubusercontent.com/bryancall/hud/main/screenshot.png?v=2)

## Features

- **GitHub Issues** — assigned open issues with labels and comment counts
- **GitHub PRs** — your open PRs with status (awaiting review, draft, merge conflict, CI failing)
- **Reviews** — PRs waiting for your review
- **Jira** — unresolved tickets assigned to you with status and priority
- **Weather** — current conditions and 3-day forecast with emoji icons
- **Clickable links** — Cmd+click any title to open it in your browser (iTerm2)
- **Auto-refresh** — updates in place like htop (default: every 60 seconds)
- **Caching** — instant startup from cache, fresh data fetched in the background
- **Themes** — 6 built-in themes (yahoo, ocean, mono, dracula, nord, solarized) plus custom JSON themes

## Requirements

- Python 3.10+
- [uv](https://docs.astral.sh/uv/) (dependencies are handled automatically via PEP 723 inline metadata)

## Install

```bash
# Clone the repo
git clone https://github.com/bryancall/hud.git
cd hud

# Copy to your scripts directory (or anywhere on your PATH)
cp hud ~/bin/hud
chmod +x ~/bin/hud
```

Or just run it directly:

```bash
./hud
```

The `#!/usr/bin/env -S uv run --script` shebang handles dependency installation automatically — no `pip install` needed.

## Setup

Run the interactive setup to configure your tokens and preferences:

```bash
hud --setup
```

You'll be prompted for:

| Setting | Description |
|---------|-------------|
| GitHub token | Personal access token ([create one here](https://github.com/settings/tokens)) with `repo` scope |
| GitHub username | Your GitHub handle |
| GitHub repos | Comma-separated list of repos to track (default: `apache/trafficserver`) |
| Jira email | Your Yahoo email for Jira API access |
| Jira PAT path | Path to your Jira personal access token (default: `~/.ssh/jira.pat`) |
| Jira base URL | Your Jira instance (default: `https://ouryahoo.atlassian.net`) |
| Jira project | Project key (default: `YTSATS`) |
| Weather zip | US zip code for weather (default: `95123`) |
| Theme | Color theme to use |

Config is stored in `~/.config/hud/config.json`.

## Usage

```bash
# Launch with auto-refresh (default: 60s)
hud

# Run once and exit
hud --refresh 0

# Custom refresh interval
hud --refresh 120

# Use a different theme
hud --theme dracula

# Re-run setup
hud --setup
```

## Themes

Six built-in themes:

| Theme | Style |
|-------|-------|
| `yahoo` | Purple and gold (default) |
| `ocean` | Deep blue and teal |
| `mono` | Grayscale |
| `dracula` | Dark purple palette |
| `nord` | Arctic blue tones |
| `solarized` | Ethan Schoonover's palette |

### Custom themes

Drop a JSON file in `~/.config/hud/themes/` with your color definitions:

```json
{
  "gold": "#ffd700",
  "border": "#6440a0",
  "issues": "#64dceb",
  "reviews": "#64f096",
  "jira": "#ffc33c",
  "prs": "#ff9180",
  "section_title": "bold #be8cff",
  "meta": "#8c82a0",
  "detail": "#b0a8c8",
  "weather_current": "bold #ffe150",
  "weather_hilo": "#e0dcf0",
  "weather_forecast": "#b4aae6"
}
```

Then use it with `hud --theme mytheme` (filename without `.json`).

## How It Works

HUD is a single Python script with no installation step. Dependencies (`rich` and `requests`) are declared inline using [PEP 723](https://peps.python.org/pep-0723/) metadata, and `uv` resolves them automatically on first run.

Data is cached to `~/.config/hud/cache.json` so the dashboard appears instantly on startup while fresh data is fetched in the background.

## License

MIT

# pi-skill-agent-browser

A [pi](https://github.com/badlogic/pi) skill package for browser automation via Chrome DevTools Protocol (CDP).

## What it does

When pi needs to browse the web — clicking buttons, filling forms, navigating pages, taking screenshots — this skill provides a lightweight set of CDP scripts that remote-control Google Chrome or Chromium. No Puppeteer, no Playwright, just raw CDP over WebSocket.

### Capabilities

| Command | Description |
|---------|-------------|
| `agent-browser start` | Launch Chrome with remote debugging on `:9222` (fresh or with your profile) |
| `agent-browser nav` | Navigate current tab or open a new tab |
| `agent-browser eval` | Execute JavaScript in the active tab |
| `agent-browser screenshot` | Capture viewport screenshot, returns temp file path |
| `agent-browser pick` | Interactive element picker (click to select, Cmd/Ctrl+Click multi-select) |
| `agent-browser dismiss-cookies` | Auto-dismiss EU cookie consent dialogs (accept or reject) |
| `agent-browser watch` | Background logging of console, errors, and network to JSONL |
| `agent-browser logs-tail` | Tail the latest log file |
| `agent-browser net-summary` | Summarize network requests/responses from logs |

### Supported cookie consent platforms

OneTrust, Cookiebot, Didomi, Quantcast, Usercentrics, TrustArc, Klaro, CookieYes, Google, YouTube, BBC, Amazon, and generic pattern matching for unlisted CMPs.

## Prerequisites

- Google Chrome or Chromium installed
- Node.js 18+

## Install

```bash
# From git (global)
pi install https://github.com/picassio/pi-skill-agent-browser

# Project-local install
pi install -l https://github.com/picassio/pi-skill-agent-browser

# From a local clone
pi install /path/to/pi-skill-agent-browser
```

After install, run `npm install` in the skill's `scripts/` directory to get the `ws` dependency:

```bash
cd <package-location>/skills/agent-browser/scripts && npm install
```

## Usage

Once installed, the skill is auto-discovered. Ask pi to browse the web:

```
> Go to https://example.com and take a screenshot
```

Or invoke explicitly:

```
/skill:agent-browser Navigate to https://news.ycombinator.com and get all article titles
```

### Quick start flow

```bash
# 1. Start Chrome
agent-browser start

# 2. Navigate
agent-browser nav https://example.com

# 3. Dismiss cookies
agent-browser dismiss-cookies

# 4. Evaluate JS
agent-browser eval 'document.title'

# 5. Screenshot
agent-browser screenshot

# 6. Pick an element interactively
agent-browser pick "Click the submit button"
```

## Structure

```
pi-skill-agent-browser/
├── package.json
├── README.md
├── LICENSE
└── skills/
    └── agent-browser/
        ├── SKILL.md
        └── scripts/
            ├── package.json          # ws dependency
            ├── cdp.js                # Minimal CDP client
            ├── start.js              # Launch Chrome
            ├── nav.js                # Navigate tabs
            ├── eval.js               # Evaluate JS
            ├── screenshot.js         # Capture viewport
            ├── pick.js               # Interactive element picker
            ├── dismiss-cookies.js    # Cookie consent handler
            ├── watch.js              # Background logger
            ├── logs-tail.js          # Tail logs
            └── net-summary.js        # Network summary
```

## Credits

Skill originally from [badlogic/pi-skills](https://github.com/badlogic/pi-skills) web-browser skill, repackaged as a standalone pi package.

## License

MIT

---
name: agent-browser
description: >
  Browse and interact with web pages by remote-controlling Google Chrome via
  Chrome DevTools Protocol (CDP). Supports navigation, JavaScript evaluation,
  screenshots, interactive element picking, cookie consent dismissal, and
  background console/network logging. Use when the user asks to browse the web,
  scrape a page, fill a form, click buttons, take screenshots, or inspect
  web content.
---

# Agent Browser

Minimal CDP tools for collaborative site exploration.

All commands use the `agent-browser` CLI pattern:

```
agent-browser <command> [args]
```

Which maps to the scripts in this skill's `scripts/` directory.

## Setup

Install the `ws` dependency before first use:

```bash
cd <skill-dir>/scripts && npm install
```

## Start Chrome

```bash
agent-browser start              # Fresh profile
agent-browser start --profile    # Copy your profile (cookies, logins)
```

Start Chrome on `:9222` with remote debugging.

## Navigate

```bash
agent-browser nav https://example.com
agent-browser nav https://example.com --new
```

Navigate current tab or open new tab.

## Evaluate JavaScript

```bash
agent-browser eval 'document.title'
agent-browser eval 'document.querySelectorAll("a").length'
agent-browser eval 'JSON.stringify(Array.from(document.querySelectorAll("a")).map(a => ({ text: a.textContent.trim(), href: a.href })).filter(link => !link.href.startsWith("https://")))'
```

Execute JavaScript in active tab (async context). Be careful with string escaping, best to use single quotes.

## Screenshot

```bash
agent-browser screenshot
```

Screenshot current viewport, returns temp file path.

## Pick Elements

```bash
agent-browser pick "Click the submit button"
```

Interactive element picker. Click to select, Cmd/Ctrl+Click for multi-select, Enter to finish.

## Dismiss Cookie Dialogs

```bash
agent-browser dismiss-cookies          # Accept cookies
agent-browser dismiss-cookies --reject # Reject cookies (where possible)
```

Automatically dismisses EU cookie consent dialogs.

Run after navigating to a page:
```bash
agent-browser nav https://example.com && agent-browser dismiss-cookies
```

Supports: OneTrust, Cookiebot, Didomi, Quantcast, Usercentrics, TrustArc, Klaro, CookieYes, Google, YouTube, BBC, Amazon, and generic pattern matching.

## Background Logging (Console + Errors + Network)

Automatically started by `agent-browser start` and writes JSONL logs to:

```
~/.cache/agent-web/logs/YYYY-MM-DD/<targetId>.jsonl
```

Manually start:
```bash
agent-browser watch
```

Tail latest log:
```bash
agent-browser logs-tail           # dump current log and exit
agent-browser logs-tail --follow  # keep following
```

Summarize network responses:
```bash
agent-browser net-summary
```

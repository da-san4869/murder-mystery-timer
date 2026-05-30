# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Running the App

No build step or server required — open files directly in a browser:

- `index.html` — Control panel (game master interface)
- `display.html` — Display screen (shown to players on a projector/secondary screen)

Both screens must be open simultaneously for the two-screen sync to work.

## Architecture

This is a **two-screen, serverless browser app** for managing timed phases in Japanese murder mystery (マーダーミステリー) events. There are no external dependencies, no bundler, and no backend.

### Screen Roles

| File | Role |
|------|------|
| `index.html` | Game master control panel — timer controls, phase navigation, settings |
| `display.html` | Player-facing projection screen — read-only, polls for state updates |

### State & Sync

All state is stored in `localStorage`. `display.html` polls every 500ms to stay in sync.

- `mmTimerData` — game configuration: title, phases array (`{name, minutes, alerts[]}`)
- `mmTimerState` — live timer state: `currentPhase`, `remaining` (seconds), `running`, `firedAlerts[]`

### Code Layout

All CSS and JavaScript is inline within each HTML file — no separate `.js` or `.css` files. `index.html` is ~940 lines; `display.html` is ~426 lines.

### Key Behaviors

- The timer runs only in `index.html`; `display.html` is purely a consumer of `localStorage`
- Alerts use the browser `speechSynthesis` API for Japanese TTS
- The display turns orange/red when ≤ 60 seconds remain in a phase
- Phase order in settings is the playback order; phases can be added, removed, or reordered from the Settings tab

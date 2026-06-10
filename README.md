# Meeting Note Recorder

Meeting Note Recorder is a desktop app that notices when you join an online
meeting, records it, and captures a live transcript — so you can stay in the
conversation instead of typing minutes. It adds a unified UI for note taking,
transcript review, and AI-assisted coaching on top.

It is the meeting recorder behind [Rebel](https://mindstone.com), Mindstone's
AI workspace, and also works perfectly well on its own. Recording and
transcription run through the [Recall.ai](https://www.recall.ai/) desktop SDK
on a bring-your-own-key basis: you supply your own Recall API key and pay
Recall as you go. No Mindstone account involved.

**What it does:**

- **Meeting detection** — spots online meetings as they start, so recording is
  not something you have to remember.
- **Recording + live transcripts** — audio/video capture via the Recall.ai
  desktop SDK, with transcripts that arrive while the meeting is still
  happening.
- **Notes in one place** — a React-based UI for note taking and transcript
  review, organised per meeting.
- **AI-assisted coaching** *(optional)* — bring an Anthropic API key for
  real-time coaching during meetings.
- **Calendar + Notion integration** *(optional)* — Google Calendar awareness
  and automated todo syncing to Notion.

**Platform support:** macOS 13+ only, for now. The Recall.ai desktop SDK and
the system audio-capture permissions this app depends on are currently wired
up for macOS.

There are no binary installers — you build from source (see below). It is less
effort than it sounds.

## Prerequisites

- macOS 13+
- Node.js 20+
- npm 10+
- Xcode Command Line Tools (`xcode-select --install`)
- Recall.ai desktop SDK account and API key
- Google Cloud project with OAuth credentials for Calendar API
- (Optional) Anthropic API key for real-time coaching features
- (Optional) Notion integration token + database IDs for automated todo syncing

## Local Setup

1. Clone the repository and install dependencies:
   ```bash
   git clone https://github.com/mindstone/meeting-note-recorder.git
   cd meeting-note-recorder
   npm install
   ```
2. Copy `.env.example` to `.env` and populate your own keys. Do **not** commit
   personal keys.
3. Build the desktop app:
   ```bash
   npm run build
   ```
4. Start the development environment:
   ```bash
   npm run dev
   ```
   This launches webpack watchers for main, preload, and renderer bundles and opens Electron pointed at `http://localhost:9000`.

## Development

### Distribution Build

- `npm run dist` creates a notarization-ready DMG at `dist-app/` (codesigning must be completed manually by whoever has signing rights).
- After copying the `.app` bundle, run `sudo codesign --force --deep --sign - /Applications/Meeting\ Note\ Recorder.app` to preserve accessibility permissions across restarts.

### Testing & QA Flow

- `npm run lint` – TypeScript lint rules across `src/` (currently emits unused-variable warnings; triage before release).
- `npm test` – Runs `test-production.js`. In non-interactive shells it only validates app start/stop; to exercise manual steps run from a terminal with the Electron window visible and follow prompts.
- Additional scripts (`test-interactive.js`, `test-production.js`, etc.) live at repo root for manual workflows.

Recent automated run (2025-10-19) reports only warnings when running non-interactively: missing log file detection and manual tests skipped. Ensure a fully interactive run before sharing release notes.

### Further docs

- [`INSTALLATION.md`](INSTALLATION.md) — detailed installation walkthrough
- [`PERMISSIONS_GUIDE.md`](PERMISSIONS_GUIDE.md) — macOS permissions setup
- [`LOGGING_GUIDE.md`](LOGGING_GUIDE.md) — logging and diagnostics
- [`TESTING.md`](TESTING.md) — testing reference

## The Mindstone open-source family

This repo is one of several open-source projects from Mindstone:

- [Rebel](https://mindstone.com) — the AI workspace desktop app this recorder
  plugs into (source release in progress).
- [mcp-servers](https://github.com/mindstone/mcp-servers) — source-available
  MCP connectors for popular SaaS tools; works with any MCP host.
- [Super-MCP](https://github.com/mindstone/Super-MCP) — a proxy MCP router that
  loads only the tools you actually need, saving your context window.
- [rebel-system](https://github.com/mindstone/rebel-system-oss) — the public
  Rebel system: skills, prompts, operators, help docs, and templates.

## License

Released under the [MIT License](LICENSE).

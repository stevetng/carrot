# CLAUDE.md

## Project Overview

Carrot is a Chrome extension that organizes open browser tabs into categories using OpenAI's GPT API. It has a vanilla JavaScript frontend (Chrome extension popup) and a Node.js/Express backend server.

## Architecture

**Client-Server pattern:**
- **Chrome Extension** (`popup.html`, `popup.js`, `popup.css`) — UI that reads open tabs via the Chrome Tabs API, sends them to the backend, and renders categorized results
- **Service Worker** (`background.js`) — Chrome extension background script
- **Backend Server** (`server.js`) — Express server exposing `POST /api/organize` that calls OpenAI GPT-3.5-turbo-16k to categorize tabs
- **Manifest** (`manifest.json`) — Chrome Extension Manifest V3 config

**Data flow:** User clicks "Organize Tabs" → extension queries Chrome Tabs API → sends tab data to `localhost:5000/api/organize` → server calls OpenAI → returns JSON categories → popup renders grouped tabs.

## File Structure

```
carrot/
├── manifest.json       # Chrome extension manifest (v3)
├── popup.html          # Extension popup UI
├── popup.js            # Popup logic (tab fetching, display, navigation)
├── popup.css           # Popup styling
├── background.js       # Extension service worker
├── server.js           # Express backend (OpenAI integration)
├── package.json        # Node.js dependencies and scripts
├── test.js             # Basic manual test (axios call)
├── example.json        # Expected output format reference
├── images/carrot.png   # Extension icon
└── .env                # OpenAI API key (not committed)
```

## Development

### Prerequisites
- Node.js and npm
- OpenAI API key in `.env` as `OPENAI_API_KEY`

### Running
```bash
npm install        # Install dependencies
npm start          # Start Express server on port 5000
```
Then load the extension as unpacked in Chrome via `chrome://extensions/` (Developer Mode).

### Scripts
- `npm start` — runs `node server.js`
- `npm test` — placeholder (not configured)

## Key Dependencies

- **express** — web server framework
- **openai** (v3) — OpenAI API client
- **cors** — CORS middleware
- **dotenv** — environment variable loading

## Code Conventions

- Vanilla JavaScript throughout (no TypeScript, no build step, no bundler)
- camelCase variable naming
- No linter, formatter, or type checking configured
- No formal test framework
- `.env` and `node_modules/` are gitignored

## Known Limitations

- Tab organization takes ~8 seconds (OpenAI API latency)
- CORS is open to all origins in the Express server
- `localhost:5000` is hardcoded in popup.js
- No automated tests or CI/CD

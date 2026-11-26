# Music Player (Windows Desktop)

A lightweight desktop music player for Windows 10/11. It searches music online, plays results, and manages a local music library. Built with Electron; the UI is plain HTML/CSS/JS with no framework.

This repository is the project homepage for the application, and also documents how the Jamendo API is used inside it.

## What it does

- **Online search and playback.** Type a song or artist name, pick a source, and results appear with cover, artist, album and duration. Click a row to play.
- **Multiple sources.**
  - iTunes Search API — 30-second preview clips.
  - Audius — full-length tracks from the Audius open catalog.
  - Internet Archive — public-domain and openly licensed audio recordings.
  - **Jamendo** — full-length tracks from the Jamendo catalog (planned integration, see below).
  - A custom endpoint option, for a user-supplied search API.
- **Local library.** Import single files or an entire folder (recursive scan), or drag audio files into the window. Supports mp3 / flac / wav / m4a / aac / ogg / opus / wma / aiff / ape.
- **Player features.** Custom playback bar, seek, volume, next/previous, shuffle, repeat, keyboard shortcuts (space to play/pause, arrow keys to seek and change volume, Ctrl+F to focus search).

## How the Jamendo API is used

The application uses the Jamendo API to let users **search the Jamendo catalog and stream the matching tracks**. Specifically:

1. The user types a query in the search box and selects the Jamendo source.
2. The app calls the Jamendo tracks endpoint with the query and a result limit, then maps the response into the app's result list (track title, artist name, album, artwork, duration, and the audio stream URL).
3. When the user clicks a track, the app plays its audio stream directly in the built-in player.
4. Every Jamendo track shown in the app displays the artist name and the album name, with a link back to the corresponding Jamendo page, in line with Jamendo's attribution requirements.

The application is a **non-commercial personal project**. It does not sell the music, does not redistribute audio files, and does not bypass any Jamendo access control. API requests are made directly from the desktop client.

## Screenshots

The player has a frameless window with a three-column layout: search and results on the left, a large now-playing stage on the right, and a playback bar at the bottom.

## Running it

End users: download the portable `.exe` and double-click. No installation required.

Developers:

```bash
cd desktop-app
npm install
npm start        # run in development
npm run dist     # build the portable exe with electron-builder
```

## Project structure

```
desktop-app/
  main.js            Electron main process: window, IPC, network requests
  preload.js         contextBridge API exposed to the renderer
  renderer/
    index.html       frameless UI shell
    style.css        dark theme styling
    app.js           search, list rendering, playback, settings
```

## Technical notes

- Electron 33, Windows x64, portable build via electron-builder.
- `contextIsolation: true`, `nodeIntegration: false`; the renderer only touches the network through a small preload API.
- All HTTP requests are made in the main process and go through Chromium's network stack, so system proxy settings are respected.

## Contact

GitHub: [@yezirong131](https://github.com/yezirong131)

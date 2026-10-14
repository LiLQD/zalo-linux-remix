# Zalo Linux Remix

A community remix of the deleted [`realdtn2/zalo-linux-2026`](https://github.com/realdtn2/zalo-linux-2026) project — an unofficial port of the Zalo macOS desktop client to Linux.

> ⚠️ **Unofficial, not affiliated with Zalo or VNG Corporation.**

## Fork status

The original upstream repository was deleted by its author. This fork preserves the working source tree and adds a small patch so the app launches cleanly without hitting a dead update endpoint.

**What changed from upstream:**

- `update.sh` is patched to exit immediately. The original update endpoint (`raw.githubusercontent.com/realdtn2/...`) no longer resolves, so every launch produced a "Zalo Update — Could not reach GitHub" dialog. Since there is no upstream to fetch from, updates are disabled.
- `start.sh` is otherwise unmodified. Version checks in the packaged Electron binary (`electron-updater`) are already dormant when running from source — they only activate in AppImage builds.

Everything else (app files, native modules, launcher logic) is byte-identical to the last upstream snapshot.

## How It Works

This is not a web wrapper. It runs the actual Zalo macOS Electron app code on Linux:

1. Extract the `.dmg` from the macOS release
2. Locate `app.asar` at `/Applications/Zalo.app/Contents/Resources/`
3. `asar extract app.asar app`
4. Run the extracted tree with **Electron 22.3.27** (`electron .`)

> Newer Electron versions break the app — v22.3.27 is required.

## Requirements

- Linux x86_64
- An internet connection (for the first Electron download if it's not already present)
- `bash`, `curl` (or `wget`), `unzip`

On Wayland compositors (Niri, Hyprland, Sway) the app runs, but you may see harmless `GetVSyncParametersIfAvailable() failed` warnings in the terminal. They come from Electron trying X11-style vsync; the app still renders correctly.

## Installation

### Clone and run

```bash
git clone https://github.com/LiLQD/zalo-linux-remix.git
cd zalo-linux-remix
./start.sh

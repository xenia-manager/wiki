# Bigscreen Mode

Bigscreen is the fullscreen, controller-friendly launcher for TV / couch use. It is a separate application project (`XeniaManager.BigScreen`) sharing the same core library, library data (`Config/games.json`), and emulator installs as the desktop app.

!!! info "Screenshot needed"
    **File:** `assets/images/bigscreen-mode.png`
    **Capture:** Bigscreen home screen with game grid focused, fullscreen.
    **Replace with:** `![Bigscreen mode](assets/images/bigscreen-mode.png)`

---

## Launching and Startup Options

- **From the desktop app**: look for the Bigscreen launch action (toolbar/page action depending on version).
- **Start in Big Screen** (`General → Start in Big Screen`, default off): boots straight into Bigscreen instead of the desktop window. See [Manager Settings](manager-settings.md#general). Turn this on for a dedicated emulation box; keep it off while you are still configuring things, since settings editing is faster in the desktop UI.
- Exiting Bigscreen returns you to the desktop (or exits entirely, depending on how it was launched).

## What You Can Do There

- Browse the same Library (artwork grid, search/sort where exposed).
- Launch games with their assigned variants — per-game settings, content, and patches apply exactly as in desktop mode.
- Basic management (content/patch/settings access depends on version — the desktop app remains the full-featured surface).

Prefer the desktop app for: first-time Xenia installation, patch editing, config tuning, profile/save surgery, and Steam shortcut creation. Use Bigscreen for: launching and playing.

## Tips

- Set up everything in desktop mode first (install emulators, scan library, verify one game boots), then switch to Bigscreen for daily use.
- If Bigscreen shows an empty library, the desktop app would too — rescan in desktop mode ([Library](library.md#scanning-and-adding-games)); both read the same `games.json`.
- Dashboard preferences persist to `Config/dashboard-settings.json` next to the other config files.

---

## Troubleshooting

- **Bigscreen starts but games fail to launch** — diagnose in desktop mode where error output and logs are visible ([Troubleshooting](../troubleshooting.md#logs)), then return to Bigscreen.
- **Controller does not navigate Bigscreen** — confirm Xenia's `gamecontrollerdb.txt` is present and your controller works in the desktop app first; Bigscreen relies on the same SDL mapping pipeline.

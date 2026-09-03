# Installation

## Prerequisites

- **Windows 10 or later** (64-bit). Xenia Manager is a Windows app (`WinExe` target).
- **[.NET 10 Desktop Runtime](https://dotnet.microsoft.com/download/dotnet/10.0)** — the Windows build is framework-dependent (`SelfContained=false`), so the runtime must be installed separately. Install the **Desktop** runtime (not just the base/console runtime), since the app has a graphical interface.
- Enough disk space for the manager plus at least one Xenia variant (each variant is a few hundred MB plus your games, content, and patches).
- (Optional) **Steam** — only needed if you want to create Steam shortcuts (see [Steam Shortcuts](guides/steam-shortcuts.md)).

!!! tip
    If Windows reports a missing `dotnet` runtime on launch, install the **.NET 10 Desktop Runtime** and try again. The Linux build story does not apply here — Xenia Manager targets Windows.

---

## Installing the Application

1. Download the latest release ZIP from the [releases page](https://github.com/xenia-manager/xenia-manager/releases/latest/).
2. Extract the archive to your preferred location (for example `C:\Tools\XeniaManager\`). The app is portable — settings, library (`Config/games.json`), emulators (`Emulators/`), and logs (`Logs/`) live next to the executable.
3. Run `XeniaManager.exe`.
4. On first startup you will see the welcome/first-run flow — continue with the [Quickstart](quickstart.md) to install Xenia and add games.

!!! note
    Keep the whole extracted folder together. Moving only `XeniaManager.exe` without its `Config/`, `Emulators/`, `Cache/`, and `Logs/` folders will make the app recreate them as empty.

---

## Folder Layout (What Gets Created)

After the first launch, next to `XeniaManager.exe` you will find:

```
XeniaManager.exe
Config/            # config.json, games.json (library), dashboard-settings.json
Emulators/         # Xenia Canary/, Xenia Mousehook/, Xenia Netplay/ (+ Content/ when unified)
Games/             # default location for game files (you can scan any folder)
GameData/          # per-game data managed by Xenia Manager
Cache/             # Images/, Database/ (x360db, patches, compatibility)
Downloads/         # temporary downloads (Xenia builds, patches)
Backup/            # profile/save backups
Logs/              # NLog log files — attach these when reporting bugs
```

Per-emulator layout (example for Canary) is documented in [Manage Xenia](guides/manage-xenia.md#emulator-folder-layout).

---

## Updating Xenia Manager

- If **Check for updates on startup** is enabled (default, see [Manager Settings](guides/manager-settings.md#updates)), the app notifies you when a new Manager release is available.
- Experimental/preview builds are tracked separately from stable builds — see [Manager Settings](guides/manager-settings.md#updates) for the **Use experimental build** toggle.
- To update manually, download the new release ZIP and extract it over your existing install, or into a fresh folder and copy your `Config/` folder across.

---

## Next Steps

- Follow the [Quickstart](quickstart.md) to install your first Xenia build and import a game.
- Want to build from source instead? See [Development](development.md).

# Manage Xenia

The Manage page installs, updates, and repairs the actual emulator builds. Xenia Manager itself is only a launcher — the games run inside one of the Xenia variants installed here.

!!! info "Screenshot needed"
    **File:** `assets/images/manage-xenia.png`
    **Capture:** Manage page showing Canary / Mousehook / Netplay rows with installed versions and Install/Update buttons.
    **Replace with:** `![Manage Xenia](assets/images/manage-xenia.png)`

---

## Variants

| Variant | Executable | Config file | Extra file | When to install it |
| ------- | ---------- | ----------- | ---------- | ------------------ |
| Xenia Canary | `xenia_canary.exe` | `xenia-canary.config.toml` | — | Default for almost every game |
| Xenia Mousehook | `xenia_canary_mousehook.exe` | `xenia-canary-mousehook.config.toml` | `bindings.ini` | Games with keyboard-and-mouse support (see [Mousehook](mousehook.md)) |
| Xenia Netplay | `xenia_canary_netplay.exe` | `xenia-canary-netplay.config.toml` | — | Games with online multiplayer support |
| Custom | you pick the `.exe` | alongside it | — | Pinning a specific build for one game (set per game in the [Details editor](library.md#game-details-editor)) |

You can install any combination — they coexist. Each game remembers which variant it launches with (`xenia_version` in `games.json`).

## Emulator Folder Layout

Each variant lives under `Emulators/<Variant>/` next to `XeniaManager.exe`:

```
Emulators/
  Xenia Canary/
    xenia_canary.exe
    xenia-canary.config.toml      # default config (Manager may move active config to config/)
    config/                       # active per-game/global configs
    content/                      # DLC/TU (or shared Emulators/Content/ when unified — see below)
    patches/                      # enabled .patch.toml files for this variant
    screenshots/                  # Xenia's own screenshots
    xenia.log                     # per-emulator log
    gamecontrollerdb.txt          # SDL controller mappings (auto-refreshed)
    xconfig.settings
  Xenia Mousehook/
    xenia_canary_mousehook.exe
    bindings.ini                  # mouse/keyboard bindings
    ...
  Xenia Netplay/
    ...
  Content/                        # shared content folder, only when "unified content" is on
```

!!! warning
    Do not rename executables or move files out of these folders by hand. The Manager locates the emulator by these exact paths. If you must intervene, use the repair/reinstall actions on the Manage page.

## Installing and Updating

### First install (1-click setup)

1. Open the **Manage** page.
2. Click **Install** next to the variant you want (start with Canary).
3. The Manager downloads the build into `Downloads/`, extracts it into `Emulators/<Variant>/`, writes the default config, and refreshes `gamecontrollerdb.txt`.
4. The row updates to show the installed version number.

### Stable vs. nightly

Each variant tracks two version strings: a **stable version** and a **nightly version**, with a **Use nightly build** toggle:

- **Stable** — less frequent, more tested snapshots.
- **Nightly** — bleeding-edge builds with the latest fixes (and the latest regressions). Prefer nightly when a game was fixed upstream yesterday, stable when you value consistency.

Flip the toggle per variant, then check for updates. The **current version** is whichever channel is selected.

### Updates

- **Automatic checks**: when update checking is enabled (see [Manager Settings](manager-settings.md#updates)), the Manager compares your installed version against the version manifest on startup and badges the Manage page when an update is available (`update_available` flag + `last_update_check_date` per variant).
- **Manual check**: use the Check-for-updates action on the Manage page.
- **Apply update**: click Update; the Manager downloads and swaps the build while preserving your `config/`, `content/`, and `patches/` folders.

### Repair and uninstall

- **Repair / Reinstall** re-downloads the current build without wiping configs or content. Use it when the executable is missing, crashes on startup for every game, or antivirus quarantined files.
- **Uninstall / Delete** removes the variant folder. Games assigned to that variant will fail to launch until you reassign them in the [Details editor](library.md#game-details-editor). Content installed in *unified* mode survives (it lives outside the variant folder — see below).

---

## Settings That Affect This Page

- **Unified content folder** (`Emulator → Unified content`, default off): when on, DLC/TU go into `Emulators/Content/` shared by all variants instead of each variant's own `content/` folder. Enable it if you run the same DLC across Canary and Netplay and do not want duplicates. See [Content](content.md#where-content-lives-unified-vs-per-emulator).
- **Check for updates on startup** and **Use experimental build** (Manager-level): see [Manager Settings](manager-settings.md#updates).
- **Automatic profile save backup** and **Profile XUID**: these live next to the emulator settings but are documented in [Profiles & Saves](profiles-saves.md).

---

## Troubleshooting

- **Install button does nothing / download fails** — check your connection and retry; downloads stage in `Downloads/` so a partial file may need deleting. See [Troubleshooting](../troubleshooting.md#xenia-download-or-update-fails).
- **Antivirus flags the emulator** — emulator builds are occasionally false-positive flagged. Restore/quarantine-exempt the `Emulators/` folder, then Repair.
- **Game launches the wrong variant** — the per-game `xenia_version` overrides the default. Fix it in the [Game Details Editor](library.md#game-details-editor), not by reinstalling.

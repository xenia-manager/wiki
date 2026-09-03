# FAQ

Frequently asked questions about Xenia Manager. If your question is not answered here, check [Troubleshooting](troubleshooting.md) or [open an issue](https://github.com/xenia-manager/xenia-manager/issues).

---

## General

### Is Xenia Manager free?

Yes. It is open source under the [BSD-3 License](https://github.com/xenia-manager/xenia-manager/blob/main/LICENSE).

### Is it affiliated with the Xenia Team?

No. Xenia Manager is **not affiliated** with the official [Xenia Team](https://xenia.jp/). It is a third-party launcher/manager for the Xenia emulator. Please do not ask the Xenia Team for Xenia Manager support (and vice versa — emulator-core bugs belong upstream).

### Does it include games, Xenia builds, DLC, or patches?

No. The Manager downloads **emulator builds** (Xenia Canary/Mousehook/Netplay) and **community metadata** (compatibility, artwork, optimized settings, patch files from public databases), but never games or copyrighted content. You must supply your own legally owned dumps, DLC, and TUs. See [Installation](installation.md) and [Quickstart](quickstart.md).

### Which Xenia version should I use?

Start with **Xenia Canary** (best general compatibility). Add **Mousehook** for keyboard-and-mouse titles and **Netplay** for online multiplayer. Assign per game — you can mix variants in one library. Details: [Manage Xenia](guides/manage-xenia.md).

### Is it safe for my PC / saves?

The Manager only writes inside its own folder (`Config/`, `Emulators/`, `Cache/`, `Backup/`, `Logs/`) plus Steam shortcuts when you ask for them. Still: enable **automatic save backup** before experimenting ([Profiles & Saves](guides/profiles-saves.md#manage-profiles)), keep a copy of `Config/games.json`, and treat nightly emulator builds as unstable by definition.

---

## Installation

### Do I need the .NET runtime?

Yes — install the **[.NET 10 Desktop Runtime](https://dotnet.microsoft.com/download/dotnet/10.0)**. The app is framework-dependent. If it refuses to start with a `dotnet`-missing error, you installed the wrong runtime variant (you need **Desktop**, not base). See [Installation](installation.md#prerequisites).

### Is there an installer? Is it portable?

Releases ship as a ZIP you extract anywhere — the app is portable, with settings and emulators stored next to `XeniaManager.exe`. See [folder layout](installation.md#folder-layout-what-gets-created).

### Can I move the Manager folder after setup?

Yes, as long as you move the **whole folder** (exe + `Config/` + `Emulators/` + the rest). Library file paths are absolute — after moving your *games* folder (not the Manager), rescan. Steam shortcuts store the old launcher path and must be recreated after moving the Manager. See [Steam Shortcuts](guides/steam-shortcuts.md#managing-shortcuts).

---

## Library & Games

### Which game formats are supported?

Disc images and extracted Xbox 360 formats handled by the built-in parsers (ISO, XEX, STFS/SVOD containers, GPD metadata, ZAR archives, and related containers). If a file will not parse here, Xenia itself would not boot it either — fix the dump. See [Library](guides/library.md#scanning-and-adding-games).

### Why is my game "Unknown Game"?

Usually an incomplete dump or an ID the database does not recognize. Work through [Unknown Games](guides/library.md#unknown-games): verify the dump, toggle Xenia-based parsing, toggle Media-ID title matching, then hand-edit details as a last resort.

### Can one entry cover multiple discs?

Yes — merge multi-disc sets into one entry with a disc picker ([Library](guides/library.md#auto-detect-and-multi-disc)). The Manager remembers the last-played disc.

### Where do playtime and last-played come from?

Tracked automatically on each launch and stored in `games.json`. Editing them by hand is possible (it is plain JSON) but unnecessary.

---

## Content, Patches, Settings

### How do I install DLC / Title Updates?

Per game via **Install Content**, without opening Xenia: [Content guide](guides/content.md). Install the TU before the DLC, and match the DLC region to the game's Title ID.

### Where do I get patches? Why is my game missing from the list?

Patches come from the Canary and Netplay community databases ([Patches](guides/patches.md#patch-sources)). If your title is absent, none are published for its Title/Media ID in that variant's database. Check you are browsing the right variant's list and that your IDs are correct.

### Should I use community optimized settings?

They are good starting points ([Xenia Settings](guides/xenia-settings.md#optimized-settings-community-presets)), applied as per-game overrides so your global config stays clean. Revert them if your hardware behaves differently.

### Global vs. per-game settings — which do I edit?

Per-game first; global only when every game benefits. A per-game override always wins over the global value, which is the #1 cause of "my setting does nothing". See [Xenia Settings](guides/xenia-settings.md#global-vs-per-game-settings).

---

## Profiles, Saves, Shortcuts

### How do I move saves to another PC?

Export the profile first, then each game's saves, then import profile-then-saves on the new machine ([Profiles & Saves](guides/profiles-saves.md#import-and-export-saves)). The Manager remaps XUIDs during the managed import — do not just copy raw folders.

### Do Steam shortcuts update when I change artwork?

No — shortcuts snapshot art at creation time. Refresh artwork, then recreate the shortcut. See [Steam Shortcuts](guides/steam-shortcuts.md#artwork).

### Can I use the Manager from the couch / TV?

Yes — [Bigscreen mode](guides/bigscreen.md). Configure everything in desktop mode first, then use Bigscreen for launching.

---

## Problems

### My problem isn't listed here — where do I start?

Head to [Troubleshooting](troubleshooting.md). If nothing helps, [open an issue](https://github.com/xenia-manager/xenia-manager/issues) and attach your log files (see [Logs](troubleshooting.md#logs)) plus the Manager version from the About page.

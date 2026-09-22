---
icon: lucide/monitor-play
---

# Steam & Desktop Shortcuts

Create Steam shortcuts for your games - with full artwork - so they launch from Steam, Big Picture, or Steam Input like native titles. Desktop shortcuts (`.lnk` files) are covered at the bottom.

Both shortcut types are **Windows-only** (hidden when not running natively on Windows, including under Wine/Proton) and ask for a disc first on multi-disc games.

---

## Creating a Shortcut

1. Select a game in the Library.
2. Open **Create Steam shortcut** (right-click → **Create Shortcut** → **Steam**).
3. The Manager generates a Steam shortcut that launches Xenia Manager's launcher path for that game (correct variant + title + disc handling included), and attaches artwork (cover and hero grid images plus the shortcut icon) resolved through the artwork manager.
4. Open Steam - the new entry appears in your library. In Big Picture / Steam Deck UI it shows with the supplied art.

!!! tip
    Steam must be installed and have been run at least once (so its shortcut file exists) before creating shortcuts. When Steam is running, the Manager restarts it itself so the new entries appear (bulk creation restarts it once).

## Artwork

- Artwork comes from the same pipeline as Library art: embedded game icons first, then downloaded artwork cached under `Cache/Images/` (title metadata lives in the database cache under `Cache/Database/`).
- "Full artwork support" means the Manager fills the Steam cover and hero grid slots (plus the shortcut icon) rather than leaving a blank grey entry. If a slot looks wrong, refresh the game's artwork in the [Details editor](library.md#game-details-editor) and recreate the shortcut.
- Custom art: replace the game's artwork first, then recreate the shortcut - shortcuts snapshot art at creation time and do not live-update when Library art changes later.

## Managing Shortcuts

- **Rename in Steam**: safe - the launch target is unchanged.
- **One shortcut per game entry**: on multi-disc merged entries you pick the disc at creation time and it is baked in as a fixed `--disc N`. Create another shortcut if you want a different disc.
- **After moving the Manager folder**: shortcuts store the launcher path. If you relocate `XeniaManager.exe`, delete and recreate affected shortcuts.
- **Removing a shortcut**: delete it in Steam (`Manage → Remove non-Steam game`). This does not touch the Library entry or game files.

---

## Troubleshooting

- **Shortcut launches but the game does not start** - the per-game `xenia_version` may point at an uninstalled variant, or the game file moved. Launch the same game inside the Manager first; fix whatever error appears there.
- **Artwork missing in Steam** - refresh Library artwork, then recreate the shortcut (Steam caches grid images aggressively; a client restart helps).
- **Duplicate shortcuts** - creating a shortcut for a title that already has one is skipped automatically. After a rename, remove the stale entry in Steam (`Manage → Remove non-Steam game`).

---

## Desktop Shortcuts (Windows-only)

**Create Desktop Shortcut** writes a `<Game Title>.lnk` file to your Desktop. It targets `XeniaManager.exe` with the game title as argument (plus `--disc N` for multi-disc picks) and uses the game's icon for the shortcut image.

Like Steam shortcuts, it stores absolute paths - recreate it after moving the Manager folder or the game files.

## Launch arguments (what shortcuts run)

Both shortcut types launch `XeniaManager.exe` with arguments parsed by the built-in CLI parser:

- `"Title"` (bare title) - which library entry to start. This is what both shortcut types generate; `--game "Exact Title"` is also accepted by the parser.
- `--disc N` (or `-d N`) - 1-based disc number for multi-disc games. Omitted → defaults to Disc 1 with no picker (the disc picker only appears for interactive launches inside the Manager).
- `--xenia_args "..."` - raw extra arguments passed through to Xenia.

Single-disc games launch directly; the loading screen, playtime tracking, and per-game config/patch handling behave exactly as in-manager launches.

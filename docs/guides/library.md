# Library

The Library page is the home screen: your scanned games, their artwork, compatibility, playtime, and every per-game action (launch, edit, content, patches, settings, shortcuts).

![Game Library](../assets/images/library-main.png)

---

## Scanning and Adding Games

### Scan a directory

1. Go to **Library → Options** (library toolbar).
2. Choose **Scan directory** and pick the folder containing your dumps.
3. The Manager walks the folder, parses each candidate with its built-in Xbox/Xenia file parsers (ISO, XEX, STFS/SVOD containers, ZAR archives), extracts Title ID / Media ID, and queries the title database. (GPD/XDBF files hold achievements/profile data, not launchable games.)
4. New games are appended to `Config/games.json`. Artwork is resolved in this order:
   1. Embedded artwork from the game file itself (XDBF SPA icon, e.g. `0x8000`) when `Use embedded artwork` is on (default).
   2. Downloaded artwork from the marketplace database (`x360db`), cached under `Cache/Database/x360db/` and `Cache/Images/`.

![Library Options](../assets/images/library-options.png)

### Auto-detect and multi-disc

- **Auto-detect new games** (on by default): when files appear in your games directory, the Manager can pick them up without a manual rescan. Toggle in [Manager Settings](manager-settings.md#library-behaviour).
- **Multi-disc games**: when two entries share a title but differ by disc (Media IDs), the Manager asks whether to merge them into one entry with a disc picker. Enable **Auto-merge multi-disc** to skip the prompt in the future.
- The merged entry remembers `last_played_disc` — the disc-selection popup pre-selects the disc you launched last time.

### What is stored per game

Each entry in `games.json` tracks:

- `game_id` (Title ID) and `media_id`, plus `alternative_id` list used for compatibility lookups
- `title` (uses Media ID for title matching when `Use MediaId for title` is on)
- `xenia_version` — which emulator variant this game launches with (Canary / Mousehook / Netplay / Custom)
- `compatibility` — cached rating from the compatibility database
- `artwork` — paths to box art / icons
- `file_locations` — game file, config, patch paths
- `playtime` (minutes), `last_played` timestamp, `last_played_disc`

---

## Views, Sorting, and Search

Toggle between **Grid view** (artwork tiles) and **List view** (table) from the Library toolbar. Your choice persists.

### Grid view options

Configurable in [Manager Settings](manager-settings.md#library-views):

- Show/hide game title on the tile (default: shown)
- Show/hide compatibility rating badge (default: shown)
- Show/hide Xenia version badge (default: hidden — useful when you mix Canary/Mousehook/Netplay per game)
- Zoom level for tile size
- Double-click tile to launch (off by default; when off, double-click opens details/selection instead)

### List view columns

Toggle each column in settings: compatibility rating, playtime, Xenia version, last played, game icon, Title ID, Media ID.

### Sort and filter

- Sort by title, playtime, last played, compatibility, and similar options; flip ascending/descending from the toolbar.
- Use the search box to filter by title or Title ID. Sorting applies to the filtered set.

---

## Right-Click Menu (Per-Game Actions)

![Game context menu](../assets/images/library-right-click.png)

Typical entries (availability depends on game state):

- **Launch** — start the game with its assigned Xenia version. For multi-disc games, pick the disc first.
- **Game Details Editor** — fix title, IDs, artwork (see below).
- **Game Settings Editor** — per-game Xenia config overrides (see [Xenia Settings](xenia-settings.md#global-vs-per-game-settings)).
- **Content Viewer / Install Content** — DLC and Title Updates (see [Content](content.md)).
- **Patches Database / Patches dialog** — game patches (see [Patches](patches.md)).
- **Mousehook Controls Editor** — only meaningful for Mousehook-assigned games (see [Mousehook](mousehook.md)).
- **Create Shortcut → Steam** — see [Steam Shortcuts](steam-shortcuts.md).
- **Open folder / Reveal files** — jump to the game file, config, or emulator folder.
- **Remove from library** — drops the entry from `games.json` (does not delete your game files).

Multi-select is supported for bulk operations; the toolbar shows the selected-games count.

---

## Game Details Editor

![Game Details Editor](../assets/images/game-details-editor.png)

Use this when a scan misidentifies a game or artwork is missing:

- **Title** — display name in the Library. Fix typos or regional naming here.
- **Title ID / Media ID / Alternative IDs** — identifiers used for database lookups. Title ID and Media ID are auto-detected and read-only in the editor; only Alternative IDs can be added manually. Wrong Alternative IDs break compatibility, artwork, and patch matching. Prefer re-scanning with `Use MediaId for title` toggled (see [Manager Settings](manager-settings.md#library-behaviour)) before adding Alternative IDs.
- **Artwork** — replace or refresh box art / icons. The Manager re-caches under `Cache/Images/`.
- **Xenia version** — which variant launches this game. Change here instead of globally when only one game needs Mousehook or Netplay.

Changes save back to `games.json` immediately.

## Game Settings Editor

![Game Settings Editor](../assets/images/game-settings-editor.png)

This edits the **per-game configuration profile**: overrides that apply only to this title, leaving the global emulator config untouched. Typical uses:

- Graphics or audio fixes that only one game needs.
- Performance tweaks (resolution scale, vsync, etc.) per title.
- Assigning a different Xenia variant or config file to this game.

The editor works on the same dynamic config model as the global [Xenia Settings](xenia-settings.md) page — every field maps to a real key in the game's `.toml` config. Delete an override to fall back to the global value.

---

## Unknown Games

### Game shows as "Unknown Game"

1. Verify the dump is complete and not corrupted (re-dump or re-copy if the file is truncated).
2. Try toggling **Parse game details with Xenia** in [Manager Settings](manager-settings.md#library-behaviour) — for some formats, launching Xenia's own parser extracts titles the fast parser misses.
3. Toggle **Use MediaId for title** and rescan.
4. As a last resort, open the **Game Details Editor** and fill in the title and IDs manually, then use **Refresh artwork**.

If the file itself will not parse at all, Xenia would not boot it either — fix the dump first.

---

## Tips

- Keep one folder per game disc and avoid renaming files inside extracted containers; the parsers rely on internal headers, not filenames.
- After moving your games folder on disk, rescan the new location and remove stale entries — stored file paths are absolute.
- Back up `Config/games.json` before bulk edits; it is a plain JSON file and easy to restore.

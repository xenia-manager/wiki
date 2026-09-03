# Troubleshooting

Common problems and how to fix them. If you are stuck, please [open an issue](https://github.com/xenia-manager/xenia-manager/issues) with your log files attached (see [Logs](#logs)) and the Manager version from the About page.

---

## Startup Problems

### App does not start / missing `dotnet` error

1. Install the **[.NET 10 Desktop Runtime](https://dotnet.microsoft.com/download/dotnet/10.0)** (the **Desktop** variant — the base runtime is not enough for a GUI app).
2. Re-run `XeniaManager.exe`.
3. If it still fails, check `Logs/` next to the executable for a startup exception, and confirm you extracted the **whole ZIP** (not just the exe).

### Window is blank / library is empty after update

- If you extracted a new release into a **fresh folder**, your library is still in the old folder's `Config/games.json`. Copy the old `Config/` folder across (app closed), then relaunch.
- If the window renders but shows no games, rescan your games directory ([Library](guides/library.md#scanning-and-adding-games)).

---

## Xenia Install & Launch

### Xenia download or update fails

1. Retry the Manage-page action — downloads stage in `Downloads/`, and a partial/failed file there can block retries. Close the Manager, delete the partial file in `Downloads/`, and try again.
2. Check disk space and antivirus quarantine (emulator builds are occasionally false-positive flagged — exempt the `Emulators/` folder, then Repair).
3. If the version manifest is unreachable (offline/proxy), the Manager cannot resolve "latest" — retry with connectivity; there is no offline install path beyond pointing a Custom game entry at an exe you already have.

### Game fails to launch (any game, any variant)

1. Confirm at least one variant shows an **installed version** on the [Manage page](guides/manage-xenia.md). A game assigned to an uninstalled variant fails immediately.
2. Launch the variant's exe directly once (double-click `Emulators/<Variant>/<exe>`) to see whether the emulator itself is broken (missing DLLs, blocked by antivirus) vs. a Manager-side path problem.
3. Check the per-emulator `Emulators/<Variant>/xenia.log` and the Manager `Logs/` (below).

### Only one game fails to launch

1. Verify the game file still exists at its stored path (right-click → open folder). Moved/renamed files produce stale `games.json` entries — rescan and remove the dead entry.
2. Check the per-game `xenia_version` in the [Details editor](guides/library.md#game-details-editor) — the game may target a variant you uninstalled.
3. Temporarily clear per-game settings overrides and disable patches to isolate the cause, then re-add them one at a time.

### Game shows as "Unknown Game"

See [Library → Unknown Games](guides/library.md#unknown-games): verify the dump, try Xenia-based parsing and Media-ID title matching, rescan, hand-edit details last.

---

## Content, Patches, Settings

### DLC or Title Update not visible in-game

1. Confirm the content was installed for the **correct Title ID** (Content Viewer lists it under the right game).
2. Install the matching **Title Update first**, then DLC.
3. If you used **Package file** install mode, reinstall as **Extracted folder** (some builds lack XContent package support).
4. Check unified vs. per-emulator folder confusion: content in `Emulators/Content/` is only read when unified mode is on; otherwise each variant reads its own `content/` folder. See [Content](guides/content.md#where-content-lives-unified-vs-per-emulator).
5. Match DLC region to the game's region/Title ID.

### Patch has no effect

1. Confirm the game launches with the variant whose `patches/` folder holds the file (Canary patches do not apply to Netplay and vice versa).
2. Confirm the patch is **enabled** in the [Configurator](guides/patches.md#patch-configurator) and the Title/Media IDs match your dump.
3. Relaunch the game fully (close the emulator window, not just reload) after toggling patches.

### Setting change does nothing

You are almost certainly editing the wrong level or variant: a **per-game override** beats the **global** value, and Canary/Mousehook/Netplay configs are independent. Open the per-game editor and the global page side by side and reconcile them. See [Xenia Settings](guides/xenia-settings.md#config-files-reference).

### Mousehook key capture records the wrong key

Close overlay/macro software that intercepts input (macro tools, some recorders, aggressive keyboard-layout switchers), retry capture, and confirm the game is assigned to the Mousehook variant with `bindings.ini` present. See [Mousehook](guides/mousehook.md).

---

## Saves & Shortcuts

### Save does not appear after import

Title-ID mismatch (wrong region/edition), wrong variant (save imported for Canary, game launches Netplay), or missing TU changing the save format. Re-export from the source setup and re-import via the managed flow — do not copy raw save folders. See [Profiles & Saves](guides/profiles-saves.md#import-and-export-saves).

### Steam shortcut is broken after moving folders

Shortcuts store absolute launcher paths. Recreate them after relocating `XeniaManager.exe` or the game files. If art is stale, recreate after refreshing artwork. See [Steam Shortcuts](guides/steam-shortcuts.md#managing-shortcuts).

---

## Logs

Attach these when reporting bugs:

```
<manager folder>/Logs/                 # Xenia Manager logs (NLog, date-rotated)
Emulators/<Variant>/xenia.log         # per-emulator log for the failing variant
```

To capture more detail, reproduce the problem, then immediately collect the newest files (timestamps matter — include what you were doing and which game/variant). Also quote the Manager version from the About page and, for game-specific issues, the Title ID / Media ID from the Details editor.

!!! tip
    Before filing, delete nothing: keep the failing `xenia.log`, the Manager log covering the same timeframe, and (for content/patch issues) a screenshot or listing of the Content Viewer / Patch Configurator state.

# Steam Shortcuts

Create Steam shortcuts for your games — with full artwork — so they launch from Steam, Big Picture, or Steam Input like native titles.

---

## Creating a Shortcut

1. Select a game in the Library.
2. Open **Create Steam shortcut** (right-click → shortcut action, or the game's details/manage actions).
3. The Manager generates a Steam shortcut that launches Xenia Manager's launcher path for that game (correct variant + title + disc handling included), and attaches artwork (capsule, hero, logo, icon) resolved through the artwork manager.
4. Open Steam — the new entry appears in your library. In Big Picture / Steam Deck UI it shows with the supplied art.

!!! tip
    Steam must be installed and have been run at least once (so its shortcut file exists) before creating shortcuts. If Steam is running, you may need to restart it to see new entries.

## Artwork

- Artwork comes from the same pipeline as Library art: embedded game icons first, then the `x360db` marketplace artwork cache (`Cache/Images/`).
- "Full artwork support" means the Manager fills the standard Steam grid slots (capsule/hero/logo/icon) rather than leaving a blank grey entry. If a slot looks wrong, refresh the game's artwork in the [Details editor](library.md#game-details-editor) and recreate the shortcut.
- Custom art: replace the game's artwork first, then recreate the shortcut — shortcuts snapshot art at creation time and do not live-update when Library art changes later.

## Managing Shortcuts

- **Rename in Steam**: safe — the launch target is unchanged.
- **One shortcut per game entry**: multi-disc merged entries produce one shortcut with disc remembering (`last_played_disc`); use the in-Manager disc picker rather than making one shortcut per disc.
- **After moving the Manager folder**: shortcuts store the launcher path. If you relocate `XeniaManager.exe`, delete and recreate affected shortcuts.
- **Removing a shortcut**: delete it in Steam (`Manage → Remove non-Steam game`). This does not touch the Library entry or game files.

---

## Troubleshooting

- **Shortcut launches but the game does not start** — the per-game `xenia_version` may point at an uninstalled variant, or the game file moved. Launch the same game inside the Manager first; fix whatever error appears there.
- **Artwork missing in Steam** — refresh Library artwork, then recreate the shortcut (Steam caches grid images aggressively; a client restart helps).
- **Duplicate shortcuts** — created twice (e.g. before and after a rename). Remove the stale one in Steam; the Manager does not deduplicate Steam-side entries.

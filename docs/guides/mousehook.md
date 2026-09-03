# Mousehook (Keyboard & Mouse)

[Xenia Mousehook](https://github.com/marinesciencedude/xenia-canary-mousehook) is the Xenia Canary fork with keyboard-and-mouse support. Xenia Manager integrates it: per-game variant assignment, bindings management, and a graphical controls editor backed by a compatibility database.

!!! info "Screenshot needed"
    **File:** `assets/images/mousehook-editor.png`
    **Capture:** Mousehook Controls Editor dialog with key/mouse bindings list.
    **Replace with:** `![Mousehook Controls Editor](assets/images/mousehook-editor.png)`

---

## Prerequisites

1. Install the **Xenia Mousehook** variant on the [Manage page](manage-xenia.md) (executable `xenia_canary_mousehook.exe`, config `xenia-canary-mousehook.config.toml`, plus `bindings.ini`).
2. Assign the game to Mousehook: open the [Game Details Editor](library.md#game-details-editor) → set **Xenia version** to **Mousehook**. Only games launched with this variant read the bindings.
3. Check the built-in **Mousehook compatibility database** (per-title support level, cached under `Cache/Database/`) — not every game has bindings, and quality varies by title.

## Mousehook Controls Editor

Open it from the game's right-click menu (**Mousehook Controls Editor**). The editor works on `bindings.ini` semantics without requiring you to hand-edit INI text:

- View the current bindings for the selected game (which keyboard key / mouse axis drives which emulated controller input).
- Rebind keys by activating the capture control and pressing the new key — the Manager uses its cross-platform Avalonia-based input listener for reliable keyboard/mouse capture.
- Reset a single binding or the whole profile to the defaults shipped in the upstream `bindings.ini` reference.
- Save. The result is written where the Mousehook build reads it for that game.

!!! note
    Bindings are inherently per-game: aiming sensitivity and button layouts that feel right in a shooter are wrong for a racing game. Avoid copying one game's bindings file onto another title unless you intend to retune from scratch.

## Input Listener Details

The "cross-platform input listener" mentioned in the feature list is the capture backend inside the Manager (keyboard/mouse detection via Avalonia, gamepad info via SDL3 bindings). You encounter it in two places:

- The Mousehook editor's **press-a-key** capture.
- Any **shortcut / hotkey** capture in settings dialogs.

If key capture misbehaves (wrong key recorded, clicks not detected), close overlay software that intercepts input (macro tools, some screen recorders) and retry — see [Troubleshooting](../troubleshooting.md#mousehook-key-capture-records-the-wrong-key).

## Workflow: Setting Up a New Game

1. Confirm the title has Mousehook support in the compatibility data.
2. Assign the game to the Mousehook variant.
3. Open the Controls Editor, load defaults, then rebind movement/aim/fire to your preference.
4. Launch and test in-game. Adjust sensitivity in-game first (where the title offers it), then fine-tune bindings — in-game sensitivity and binding scale multiply.
5. Optionally apply an [optimized settings](xenia-settings.md#optimized-settings-community-presets) preset for the title; some presets assume Mousehook builds.

---

## Troubleshooting

- **Bindings have no effect** — the game is almost certainly not launching with the Mousehook variant. Verify the per-game assignment and that `bindings.ini` exists in `Emulators/Xenia Mousehook/`.
- **Mouse works but keyboard does not (or vice versa)** — check for conflicting per-game config overrides capturing the device exclusively, and retry capture with overlays disabled.
- **No bindings published for my game** — start from the upstream default `bindings.ini` and build bindings manually; consider contributing them back to the Mousehook project.

# Development

This page covers building Xenia Manager from source and navigating the codebase. For full coding standards, naming conventions, theme authoring, and translation steps, see the canonical docs in the main repository: [`docs/CONTRIBUTING.md`](https://github.com/xenia-manager/xenia-manager/blob/main/docs/CONTRIBUTING.md) and [`docs/TRANSLATIONS.md`](https://github.com/xenia-manager/xenia-manager/blob/main/docs/TRANSLATIONS.md).

---

## Prerequisites

- [**.NET 10 SDK**](https://dotnet.microsoft.com/download/dotnet/10.0) (SDK, not just the Desktop Runtime — you need `dotnet build`/`test`).
- Python 3.x (for `scripts/lint.py` and translation tooling).
- Git. Clone the main repo (not this wiki):
  ```bash
  git clone https://github.com/xenia-manager/xenia-manager.git
  cd xenia-manager
  ```

## Building and Testing

From the repository root (where `Xenia Manager.sln` lives):

```bash
dotnet restore
dotnet build
dotnet test
```

- Solution: `Xenia Manager.sln` (version is stamped via root `Directory.Build.props`).
- CI produces release ZIPs automatically; experimental builds are tracked on a separate board/channel from stable (see [Manager Settings](guides/manager-settings.md#updates)).
- Tests live in `tests/` next to `source/`, one area per project where applicable.

## Project Structure

The solution follows a multi-project layout under `source/`. Business logic lives in the libraries so both UI surfaces (desktop + BigScreen) share it:

| Project | Role |
| ------- | ---- |
| `XeniaManager` | Main desktop app: Views, ViewModels, UI services (Avalonia + FluentAvalonia). Pages: Library, Manage, XeniaSettings, Settings, About. Keep code-behind minimal; logic goes in ViewModels/core |
| `XeniaManager.BigScreen` | Fullscreen TV launcher sharing the core libraries (see [Bigscreen](guides/bigscreen.md)) |
| `XeniaManager.Core` | Business logic: `Manage/` (Game, Config, Patch, Content, Profile, Save, Shortcut, Artwork, Launcher managers), `Models/`, `Settings/`, `Installation/`, `Utilities/` |
| `XeniaManager.Database` | Online database clients + models: Xbox marketplace (`x360db`), compatibility (Canary/Mousehook/Netplay), patches (Canary/Netplay), optimized settings |
| `XeniaManager.Files` | File-format parsers + models for Xbox/Xenia types (ISO, XEX, STFS, SVOD, GPD, ZAR, etc.) |
| `XeniaManager.Logging` | NLog-based logging infrastructure (`Logger` class with Trace → Fatal levels) |

Key runtime files (next to the built exe): `Config/config.json` + `games.json`, `Emulators/<Variant>/`, `Cache/Database/` + `Cache/Images/`, `Logs/`, `Backup/`, `Downloads/`. Constants live in `XeniaManager.Core/Constants/` (`AppPaths.cs`, `XeniaPaths.cs`, `Urls.cs`).

## Coding Standards (Summary)

Full rules are in `CONTRIBUTING.md` (and encoded in root `.editorconfig`, enforced by `python scripts/lint.py` with ReSharper `cleanupcode`):

- **MVVM** with CommunityToolkit.Mvvm (`[ObservableProperty]`, `On<Property>Changed` partials). Views stay lightweight.
- **Naming**: `PascalCase` methods/properties; `_camelCase` private fields; `camelCase` locals; Hungarian prefixes for AXAML elements (`Cmb`, `Txt`, `Btn`, `Tbl`, `Sp`, `Grd`, `Sv`, `Exp`) with the prescribed attribute order.
- **Formatting**: 4 spaces, braces on new lines, file-scoped namespaces, explicit types (no `var`, no target-typed `new()`), lines under 160 chars, alphabetized `using`s (system first).
- **Docs**: XML doc comments on public/internal types and members; sparse inline comments.
- **Logging**: `try/catch` + `Logger.Error<T>` / `Logger.LogExceptionDetails<T>`; never swallow exceptions silently.

### Lint

```bash
python scripts/lint.py              # format everything
python scripts/lint.py --check      # verify only (what CI runs)
python scripts/lint.py --include "source/XeniaManager.Core/**/*.cs"  # scoped (repeatable)
python scripts/lint.py --changed    # staged + unstaged + untracked
python scripts/lint.py --staged     # staged only
```

### Commits and PRs

- Branches: `feature/...`, `bugfix/...`, `refactor/...`, `docs/...`.
- Commits: conventional prefix style, e.g. `[Feature] Add game details editor dialog`, `[Bugfix] Fix crash on corrupted library file`. Keep commits atomic.
- PRs target the **`dev`** branch with what/why/testing described, related issues linked, and screenshots for UI changes.

## Translations

Summary of `TRANSLATIONS.md` (read the full file before starting):

1. Fork + branch `translation/<code>` (codes are .NET culture codes, e.g. `es`, `fr`, `de`).
2. Copy `source/XeniaManager/Resources/Language/en.axaml` → `<code>.axaml`.
3. Translate only the text between `<sys:String>` tags. Keep every `x:Key`, every placeholder (`{0}`, `{1}`), and escapes like `&#10;` intact.
4. Optionally build locally and preview (do **not** commit changes to the `SupportedLanguages` array — maintainers wire that at release).
5. Commit (`Add <Language> translation (<code>)`), push, open a PR with language + proficiency notes.

Progress is tracked by the translation chart tooling (`scripts/generate_translation_progress.py`, `check_localization.py`, `sync_localization.py`).

## Themes

Summary of the `CONTRIBUTING.md` theme section:

1. Copy `source/XeniaManager/Resources/Themes/Template.axaml` → `MyCustomTheme.axaml`.
2. Recolor values, keeping all `x:Key` names unchanged (dark themes: dark backgrounds + light text; light themes: inverse; keep WCAG AA contrast; pick accents that work on both).
3. Register in `source/XeniaManager.Core/Models/Theme.cs` (enum) and the theme dictionary in `ThemeService.cs` (`ResourcePath`, `BaseTheme`, optional `FallbackTheme`).
4. Build, load every control type, and verify.

Built-in themes are `System`, `Light`, `Dark`, `Amoled`, `Steam` (see `Theme.cs` — the source of truth if the guide ever disagrees).

---

## This Wiki (Docs Tooling)

This wiki lives in a separate repo (`xenia-manager/wiki`) and is built with [Zensical](https://zensical.org/) from Markdown in `docs/`, deployed to GitHub Pages (`gh-pages` branch). Navigation is declared in `zensical.toml` (`nav = [...]`).

### Preview locally with `uv`

The project uses `uv` for the docs virtualenv (mirroring the DualSenseClient docs setup):

```bash
git clone https://github.com/xenia-manager/wiki.git
cd wiki
uv venv
uv pip install -r requirements.txt   # installs `zensical`
uv run zensical serve                # preview at http://127.0.0.1:8000
```

### Build

```bash
uv run zensical build --clean --strict
```

Output goes to `site/` (git-ignored). `--strict` fails on warnings — fix broken links and missing pages before pushing.

### Contributing to docs

- Edit Markdown under `docs/`; add new pages to the `nav` array in `zensical.toml`.
- Screenshot placeholders use this convention (greppable, renders as an info box):
  ```markdown
  !!! info "Screenshot needed"
      **File:** `assets/images/<name>.png`
      **Capture:** what to capture.
      **Replace with:** `![Alt](assets/images/<name>.png)`
  ```
- Run the strict build locally, then open a PR against `main` — `gh-pages` updates automatically on merge.

# Xenia Manager Wiki

[![Documentation](https://img.shields.io/badge/Documentation-Online-2E3440?style=for-the-badge)](https://xenia-manager.github.io/wiki/)
[![Zensical](https://img.shields.io/badge/Built%20with-Zensical-2E3440?style=for-the-badge)](https://zensical.org/)
[![License](https://img.shields.io/github/license/xenia-manager/wiki?style=for-the-badge&label=License&color=2E3440)](LICENSE)

> **Note:** This wiki is **not affiliated** with the official [Xenia Team](https://xenia.jp/). It documents [Xenia Manager](https://github.com/xenia-manager/xenia-manager), which is itself not affiliated with the official Xenia Team.

---

**Xenia Manager Wiki** is the official documentation site for [Xenia Manager](https://github.com/xenia-manager/xenia-manager) — a user-friendly tool designed to simplify the use of the [Xenia Emulator](https://xenia.jp/). This wiki covers setup, game management, patches, and configuration.

Documentation is built with [Zensical](https://zensical.org/) and deployed to GitHub Pages via the `gh-pages` branch.

---

## Table of Contents

- [Documentation](#documentation)
- [Quickstart](#quickstart)
- [Contributing](#contributing)
- [Credits](#credits)
- [License](#license)

---

## Documentation

Browse the live wiki at **[https://xenia-manager.github.io/wiki/](https://xenia-manager.github.io/wiki/)**.

> Currently **Coming soon** — content is being migrated and expanded.

---

## Quickstart

### Prerequisites

- [Python 3.x](https://www.python.org/)
- [Zensical](https://zensical.org/) (`pip install zensical`)

### Run locally

1. Clone the repository:
   ```bash
   git clone https://github.com/xenia-manager/wiki.git
   cd wiki
   ```
2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
3. Start the preview server:
   ```bash
   zensical serve
   ```
   Open `http://127.0.0.1:8000` in your browser.

### Build

```bash
zensical build --clean --strict
```

Output is generated in `site/` (ignored via `.gitignore`).

---

## Contributing

We welcome contributions! To add or edit documentation:

- Edit Markdown files in `docs/`
- Update navigation in `zensical.toml` if you add new pages
- Run `zensical build --strict` locally to verify no warnings
- Open a pull request against `main` — the `gh-pages` branch is updated automatically on merge

See the main project's [Contributing Guide](https://github.com/xenia-manager/xenia-manager/blob/main/docs/CONTRIBUTING.md) and [issues](https://github.com/xenia-manager/xenia-manager/issues) for broader context.

---

## Credits

### Project

- [Xenia Manager](https://github.com/xenia-manager/xenia-manager) — main application
- [Zensical](https://zensical.org/) — static site generator (built by the creators of Material for MkDocs)

---

## License

This project is licensed under the [BSD-3 License](LICENSE).

---

**Thank you for using and supporting Xenia Manager!**

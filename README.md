# commitsense
Stop writing commit messages by hand. CommitSense uses local Ollama AI to turn any git diff into a Conventional Commit + PR description in seconds. 100% offline (your code never leaves your machine). Free, MIT, pip-installable. 13 flags, disk cache, smart truncation, git hook installer, 60+ tests.
# 🧠 CommitSense

> AI-powered Conventional Commit messages using **local Ollama** — no cloud, no leaks, no API keys.

[![CI](https://github.com/<your-username>/commitsense/actions/workflows/ci.yml/badge.svg)](https://github.com/<your-username>/commitsense/actions/workflows/ci.yml)
[![Python 3.8+](https://img.shields.io/badge/python-3.8%2B-blue)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/license-MIT-green)](LICENSE)
[![Ollama](https://img.shields.io/badge/powered%20by-Ollama-black)](https://ollama.com)

CommitSense reads your staged (or unstaged) git diff, asks a **local** LLM to summarise it, and writes a perfect `type(scope): description` commit message plus a ready-to-paste PR description.

![CommitSense in action](docs/demo.gif)

### Before vs After

![Same code, cleaner history](docs/assets/comparison.png)

---

## ✨ Features

- 🪄 **One command** — `commitsense` does everything: diff → AI → preview → commit
- ⚡ **Fast** — disk-cached responses, smart diff truncation, lazy Ollama health checks
- 🧱 **Strict** — auto-enforces Conventional Commits spec, detects breaking changes
- 🛡️ **Offline** — runs 100% locally via Ollama (your code never leaves the machine)
- 🎨 **Beautiful** — colour-coded output, animated spinner, diff stats
- 🌍 **Multi-language** — `commitsense --lang hindi` (Hindi, Spanish, French, German…)
- 🪝 **Git-hookable** — `commitsense --install-hook` for zero-friction usage
- 🧪 **Battle-tested** — 63 unit tests, full CI matrix (Linux/macOS/Windows × py 3.8-3.13)
- ✏️ **Editable** — review, edit, or reject the message before committing
- 🔁 **Amend-aware** — `commitsense --amend` to rewrite your last commit
- 💾 **Dry-run** — `commitsense --dry-run` to preview without committing

---

## 📦 Installation

### 1. Install Ollama
```bash
# macOS / Linux
curl -fsSL https://ollama.com/install.sh | sh

# Windows: download from https://ollama.com/download

# Pull a model
ollama pull llama3
```

### 2. Install CommitSense
```bash
git clone https://github.com/<your-username>/commitsense.git
cd commitsense
pip install -e .
```

Or directly:
```bash
pip install git+https://github.com/<your-username>/commitsense.git
```

Now `commitsense` is on your `PATH` from any directory.

---

## 🚀 Quick start

```bash
cd any/git/repo
commitsense
```

That's the whole flow. You'll see:

```
╔════════════════════════════════════════╗
║   🧠 CommitSense v1.1.0                ║
║   AI-powered Conventional Commits      ║
╚════════════════════════════════════════╝

🔍 Found changes in 3 file(s) (staged).
   +124 -37 across 3 files
🤖 Asking llama3 to summarise the diff…
  ⠹ Ollama is thinking… (attempt 1)

✅ Suggested commit message:
   feat(auth): add google oauth with token refresh

📝 PR Title:
   Add Google OAuth login system

📄 PR Description (preview):
   ## What changed
   ...

What do you want to do?
> ✅ Commit with this message
  ✏️  Edit message manually then commit
  🔄 Regenerate (try again)
  📋 Copy message to clipboard
  💾 Save PR description to pr_description.md
  ❌ Cancel
```

---

## 🎛️ All flags

| Flag | Short | Description |
|------|-------|-------------|
| `commitsense` | — | Run the full flow |
| `--config` | — | Edit saved settings interactively |
| `--model NAME` | `-m` | Override Ollama model for this run |
| `--lang LANG` | — | Output language (`english`, `hindi`, `spanish`, `french`, `german`) |
| `--stage-all` | — | Run `git add -A` first |
| `--amend` | — | Amend the previous commit instead of creating new |
| `--yes` | `-y` | Skip menu, commit immediately |
| `--dry-run` | — | Show message, don't commit |
| `--no-cache` | — | Bypass the response cache |
| `--no-color` | — | Disable ANSI colours (for piping/CI) |
| `--verbose` | `-v` | Show debug info |
| `--check "MSG"` | — | Lint an existing message, no AI call |
| `--clear-cache` | — | Wipe on-disk cache and exit |
| `--install-hook` | — | Install a `prepare-commit-msg` git hook |
| `--version` | — | Print version |
| `--help` | `-h` | Full help |

---

## 🪝 Git hook (auto-fill on `git commit`)

```bash
commitsense --install-hook
```

Now every `git commit` will auto-suggest a message. Bypass with `git commit --no-verify`.

---

## ⚙️ Configuration

Config file: `~/.commitsense/config.json`

```json
{
  "model": "llama3",
  "auto_stage_all": false,
  "commit_style": "conventional",
  "language": "english"
}
```

Edit interactively:
```bash
commitsense --config
```

Cache lives at `~/.commitsense/cache.json` (SHA-256 keyed, 1-week TTL, max 64 entries).

---

## 🔄 Switching Ollama models

Any model that works with `/api/generate` is supported:

```bash
ollama pull mistral       && commitsense --model mistral
ollama pull codellama     && commitsense --model codellama
ollama pull phi3          && commitsense --model phi3
ollama pull deepseek-coder && commitsense --model deepseek-coder
```

Set permanently:
```bash
commitsense --config      # change "model"
```

---

## 🧪 Lint an existing message

```bash
$ commitsense --check "feat: Add Login."
Commit message has issues:
   ⚠️  description should start lowercase
   ⚠️  description should not end with a period
$ echo $?
1
```

---

## 🩹 Troubleshooting

| Problem | Fix |
|---------|-----|
| `❌ Ollama not running` | Run `ollama serve` in another terminal |
| `❌ Model 'llama3' not found` | Run `ollama pull llama3` |
| `ModuleNotFoundError: distutils` (Py 3.12+) | `pip install --upgrade setuptools` |
| Clipboard doesn't work on Linux | `sudo apt install xclip` (or `xsel`) |
| Inquirer glitches on Windows | Use **Windows Terminal** (not cmd.exe) or run in WSL |
| AI ignores scope/types | Try `commitsense --model mistral` or `--no-cache` |

---

## 🧑‍💻 Development

```bash
git clone https://github.com/<your-username>/commitsense.git
cd commitsense
python -m venv .venv && source .venv/bin/activate
pip install -e ".[dev,test]"

pytest                          # run all tests with coverage
pytest tests/test_formatter.py  # single file
pytest -k conventional          # by keyword
ruff check commitsense tests    # lint
mypy commitsense                # type-check
```

---

## 🤝 Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md). PRs welcome — dogfood the tool:

```bash
git checkout -b feature/awesome
git add .
commitsense       # → feat: add awesome thing
git push origin feature/awesome
```

---

## 📜 License

[MIT](LICENSE) © CommitSense Contributors

---

## 🌐 Landing page

A static landing page lives at [`docs/index.html`](docs/index.html) — enable GitHub Pages on the `main` branch to publish at `yourname.github.io/commitsense`.

---

## 🌟 Star history

If CommitSense saved you time, drop a ⭐ — it helps others find it.

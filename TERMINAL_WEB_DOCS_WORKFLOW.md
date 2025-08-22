# Terminal Web/Docs Workflow

A practical workflow for reading documentation from the terminal on macOS using lightweight CLI tools.

- Browsers (non-JS): w3m, lynx, links/elinks
- Search from terminal: ddgr (DuckDuckGo)
- CLI examples (tldr client): tealdeer (command: `tldr`)
- Offline API docs: dasht (Dash/Zeal docsets)

Note: Browsh has been removed from Homebrew; use a GUI browser for JS-heavy sites when necessary.

---

## Install (Homebrew)

```bash
brew install w3m lynx links elinks dasht ddgr tealdeer
```

---

## What each tool does

- w3m: Fast text browser with decent rendering, follows links, supports tables, inline images in some terminals.
- lynx: Longstanding text browser with robust navigation; no JavaScript.
- links / elinks: Similar to lynx; elinks adds tabs and richer UI.
- ddgr: DuckDuckGo search from the terminal; can open results in a browser.
- tealdeer (tldr): Concise, example-first docs for CLI tools. Replaces deprecated Homebrew `tldr`.
- dasht: Offline API/docset search and browse (uses Dash/Zeal docsets). Integrates with w3m.

---

## Quick workflows

### 1) Search → read in terminal

```bash
# Search the web and open results in w3m when selected
BROWSER=w3m ddgr 'golang http client example'
```

Tips:
- ddgr prompts to open a result; with `BROWSER=w3m` it opens inside the terminal.
- Use quotes for multi-word queries.

### 2) CLI examples fast (tldr/tealdeer)

```bash
# First run may refresh cache automatically
tldr tar
tldr grep
```

Tips:
- Short, example-heavy pages. Use for quick reminders and flags.

### 3) Offline API docs (dasht)

```bash
# Install some docsets (examples)
dasht-docsets-install go go-moderngolang python_3 mdn css

# Search and browse
# Search across all installed docsets
dasht http client

# Restrict to a docset
dasht go http
```

Notes:
- dasht will render pages in your terminal via w3m by default (installed as a dependency).
- Use arrow keys/Enter in w3m; `q` quits.

### 4) Browse docs directly (w3m / lynx)

```bash
# w3m
ew3m https://pkg.go.dev/net/http

# lynx
lynx https://developer.mozilla.org/
```

w3m keys: arrows to move, Enter open, Shift-B back, `/` search, `q` quit.
lynx keys: arrows navigate, `g` goto URL, `/` search, `q` quit.

---

## Handy aliases (zsh)

Append to ~/.zshrc:

```bash
# Use w3m for ddgr open
export BROWSER=w3m

# Quick open with w3m
alias wdoc='w3m'

# ddgr shortcuts
alias s='ddgr'
alias sg='ddgr site:github.com'

# dasht helpers
alias dsearch='dasht'
```

Reload shell after editing:

```bash
source ~/.zshrc
```

---

## Choosing the right tool

- Need speed and simple rendering: w3m
- Most compatible without JS: lynx (or links/elinks)
- Task-focused CLI examples: tealdeer (`tldr <cmd>`)
- Offline API browsing across stacks: dasht (+docsets)
- Web search from terminal: ddgr (+ `BROWSER=w3m`)

JS-heavy sites: open in your GUI browser when w3m/lynx fail to render usefully.

---

## Cheatsheet

w3m:
- Navigation: arrows, Enter open link, Shift-B back
- Search in page: `/`
- Quit: `q`

lynx:
- Navigation: arrows, Enter open link
- Go to URL: `g`
- Search in page: `/`
- Quit: `q`


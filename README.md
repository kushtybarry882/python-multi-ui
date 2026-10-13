# Interface Styles

Copy-paste-ready source code for every UI component used across the
[Python Multi-Tool Toolkit](https://github.com/kayden765/python-multitool-main).
Each `.txt` file contains the **complete, real source code** for that UI module
— not a template or stub. Simply rename to `.py` and drop into your project.

## File Map

| File | Source Module | What It Provides |
|------|---------------|-----------------|
| `putty ui code.txt` | `core/ui/putty.py` | `PuttyUI` — PuTTY-style login banner, ANSI menus, panels, tables, prompts (with optional Rich fallback) |
| `multi ui code.txt` | `core/ui_theme.py` | 19 banner themes, `UI_MAP`, `DIR_THEME_STYLES`, theme persistence via `config.json`, `handle_customize()` |
| `multi ui code FOR TABS.txt` | `core/dir_banners.py` | Directory banner art (`_art_*` functions), `DIR_ITEMS`, `DIR_TITLES`, `MARKERS`, `render_directory_ui()` |

> **Tip:** All three files use ANSI 256-color escape codes (`\033[...m`).
> On Windows, call `os.system('')` at startup to enable VT processing.

---

## Quick Start (3 Steps)

```bash
# 1. Clone
git clone https://github.com/kayden765/python-multitool-main.git
cd python-multitool-main

# 2. Install dependencies
pip install -r setup/requirements.txt

# 3. Launch
python mainframe.py
```

---

## Detailed Setup

### Prerequisites

| Requirement | Minimum Version | Install Command |
|-------------|----------------|-----------------|
| Python | 3.8+ | [python.org](https://python.org) |
| pip | 21.0+ | bundled with Python |
| Git | any | [git-scm.com](https://git-scm.com) |
| Windows | 10/11 | (recommended) |
| Rich | 13.0+ | `pip install rich` |

> **Windows users:** For full ANSI color support, either
> 1. Use **Windows Terminal** (recommended), or
> 2. Enable VT mode at the top of `mainframe.py`:
>    ```python
>    import os
>    if os.name == 'nt':
>        os.system('')
>    ```

### Step 1 — Clone the Repository

```bash
git clone https://github.com/kayden765/python-multitool-main.git
cd python-multitool-main
```

### Step 2 — Install Python Dependencies

```bash
pip install -r setup/requirements.txt
```

The full dependency list lives in `setup/requirements.txt`. Core packages:

| Package | Purpose |
|---------|---------|
| `rich` | Rich terminal rendering (tables, panels, text) |
| `colorama` | Cross-platform ANSI color init (Windows) |
| `requests` | HTTP client for API calls |
| `psutil` | System metrics |

> **No external tools are required to start.** All UI modules in this folder
> are pure Python / pure ANSI.

### Step 3 — Configure the Tool (Optional)

A `config.json` in the project root controls runtime settings:

```json
{
    "public_host": "0.0.0.0",
    "public_port": 8080,
    "listen_port": 8080,
    "ui_theme": "1"
}
```

| Key | Description | Default |
|-----|-------------|---------|
| `ui_theme` | UI theme index (1–19). See `multi ui code.txt` → `UI_MAP` | `"1"` |
| `public_host` | Public-facing IP | `"0.0.0.0"` |
| `public_port` | External communication port | `8080` |
| `listen_port` | Local listener port | `8080` |

### Step 4 — Launch

```bash
python mainframe.py
```

You should see the PuTTY-style banner followed by the main directory menu:

```
  [01] Sub-Directory 01 // Network Infrastructure & Endpoint Recon Cores
  [02] Sub-Directory 02 // External OSINT & Target Record Profilers
  ...
  [10] Terminate Active Mainframe Operator Control Session
```

Use number keys to navigate. Press **ESC** to return to the previous menu.

---

## Using the Interface Styles

### PuTTY UI (`putty ui code.txt`)

The default terminal UI. Drop `putty.py` into your `core/ui/` package alongside
`console.py` and `__init__.py`.

```python
from core.ui import get_ui  # or from core.ui.putty import PuttyUI

ui = PuttyUI()
ui.banner()
ui.main_menu()

# Get user input
command = ui._input("operator@mainframe:~# ")

# Show results
ui.success("Scan complete")
ui.keyval_table("Results", [
    ["Host", "192.168.1.1"],
    ["Port", "80"],
    ["Status", "Open"],
])
ui.pause()
```

### Theme Switching (`multi ui code.txt`)

Theme persistence uses `config.json` with the key `ui_theme`.

```python
from core.ui_theme import load_theme, save_theme, show_theme, handle_customize

# Load saved theme (returns "1" if no config)
current_theme = load_theme()

# Display the banner for a theme
show_theme("13")  # Matrix glitch theme

# Show interactive theme picker
new_theme = handle_customize(current_theme)

# Save selection
save_theme(new_theme)
```

### Directory Banners (`multi ui code FOR TABS.txt`)

Renders themed banners + item lists for each sub-directory (01–09).

```python
from core.dir_banners import get_dir_banner

# Returns a full banner string (or None for the plain theme)
banner = get_dir_banner("01", "7")  # Dir 01, Cyberpunk theme
print(banner)
```

---

## Available Themes

Themes 1–19 (defined in `multi ui code.txt` → `UI_MAP`):

| # | Name | Color |
|---|------|-------|
| 1 | Terminal UI (Default) | Green |
| 2 | Planet UI | Cyan |
| 3 | Mainframe UI | Green |
| 4 | Reaper UI | Red |
| 5 | Hitla UI | Magenta |
| 6 | Matrix UI | Green |
| 7 | Cyberpunk 2077 UI | Neon Pink |
| 8 | Ghost Shell UI | White |
| 9 | Devil Core UI | Red |
| 10 | Retro 80s Synth UI | Orange |
| 11 | Neon Hack UI | Lime |
| 12 | Deep Space Node UI | Purple |
| 13 | Medieval UI | Yellow |
| 14 | Steampunk UI | Orange |
| 15 | Synthwave UI | Neon Pink |
| 16 | Wasteland UI | Red |
| 17 | Corporate UI | White |
| 18 | Abyssal UI | Cyan |
| 19 | Void UI | Purple |

---

## PuTTY UI Methods

| Method | Description |
|--------|-------------|
| `ui.banner()` | Print the PuTTY-style connection banner |
| `ui.main_menu()` | Display the 10-item main directory menu |
| `ui.submenu(title, items, back_text)` | Display a submenu with colored items |
| `ui.directory_menu(title, items, header_color)` | Display a numbered directory listing |
| `ui.info(msg)` | Print `[*]` cyan info message |
| `ui.success(msg)` | Print `[+]` green success message |
| `ui.warning(msg)` | Print `[!]` yellow warning message |
| `ui.error(msg)` | Print `[X]` red error message |
| `ui.section(title, color, subtitle)` | Print a section header with divider |
| `ui.keyval_table(title, rows, border_style)` | Print a key-value result table |
| `ui.result_table(title, headers, rows, border_style)` | Print a column-based result table |
| `ui.panel(content, title, ...)` | Print an ASCII-bordered panel |
| `ui.prompt_input(prompt, default)` | Prompt for input with optional default |
| `ui.pause(msg)` | Pause and wait for Enter |
| `ui.clear()` | Clear the terminal screen |

---

## Customization

Every `.txt` file includes `# INPUT TAB HERE` and `# EXAMPLE:` comments
showing where to insert your own tools, banners, or theme entries.
Follow these placeholder patterns when extending the UI.

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| No colors / garbled text | Run on Windows Terminal, or ensure `os.system('')` is called |
| `ModuleNotFoundError: No module named 'rich'` | `pip install rich` (the UI falls back to ANSI if missing) |
| Theme not loading | Check `config.json` has `"ui_theme": "1"` |
| Banner looks misaligned | Ensure your terminal supports UTF-8 |

---

## Tutorial

Setup and usage tutorials for every UI component and module:
https://www.youtube.com/@kydn-iu5kd

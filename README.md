# Nuke Tools Manager
## Full PySide2 / PySide6 Tool Manager for Nuke 13–17+

---

## Files

| File | Purpose |
|---|---|
| `pyside_compat.py` | Auto-selects PySide2 or PySide6 based on Nuke version |
| `zip_utils.py` | Zip, unzip, install, uninstall, manifest management |
| `tool_manager_ui.py` | Full 4-tab GUI — run standalone OR from inside Nuke |
| `init.py` | Copy to `~/.nuke/` — auto-registers tools on Nuke startup |
| `menu.py` | Copy to `~/.nuke/` — adds "Tools Manager" to Nuke's menubar |

---

    File / Folder,Purpose
    pyside_compat.py,Auto-selects PySide2 or PySide6 based on Nuke version
    zip_utils.py,"Zip, unzip, install, uninstall, manifest management"
    tool_manager_ui.py,Full 4-tab GUI — run standalone OR from inside Nuke
    init.py,Copy to ~/.nuke/ — auto-registers tools on Nuke startup
    menu.py,"Copy to ~/.nuke/ — adds ""Tools Manager"" to Nuke's menubar"
    launch_nk_tools.py,External launcher script to open the tools outside of Nuke
    sound_utils.py,Handles audio feedback and sound effects for UI actions
    README.md,Documentation and installation instructions
    "icons/, logo/, icon_256.png","UI icons, tool graphics, and application visuals"
    sounds/,Directory containing audio files used by the UI
    setting_tip/,Directory containing tooltip data or settings-related assets

## Nuke Version → PySide

| Nuke | Python | PySide |
|---|---|---|
| 13.x | 3.7 | PySide2 |
| 14.x | 3.9 | PySide2 |
| 15.x | 3.10 | PySide2 |
| 16.x | 3.11 | PySide6 |
| 17.x | 3.11 | PySide6 |

---

## Installation

### Step 1 — Copy the tools folder .nuke folder  paste  

# init.py registered  &  nuke restart

    nuke.pluginAddPath(r'./nuke_tools_manager')

# Tools folder structure  


```
~/.nuke/
├── init.py                  ← copy from this package (or merge)
├── menu.py                  ← copy from this package (or merge)
└── nuke_tools_manager/
    ├── icons/
    ├── logo/
    ├── setting_tip/
    ├── sounds/
    ├── icon_256.png
    ├── launch_nk_tools.py
    ├── pyside_compat.py
    ├── README.md
    ├── sound_utils.py
    ├── tool_manager_ui.py
    └── zip_utils.py
```

### Step 2 — Install PySide (if not already present)

```bash
# For Nuke 13/14
pip install PySide2

# For Nuke 15/16
pip install PySide6
```

Or using Nuke's bundled Python:

```bash
# Find Nuke's Python path, e.g.:
/Applications/Nuke15.0v1/Nuke15.0v1.app/Contents/MacOS/python -m pip install PySide6

/usr/local/Nuke15.0v1/python -m pip install PySide6

"C:\Program Files\Nuke15.0v1\python.exe" -m pip install PySide6


macOS: /Users/<your_username>/.nuke

Linux: /home/<your_username>/.nuke

Windows: C:\Users\<your_username>\.nuke


```

### Step 3 — Launch

**From Nuke's Script Editor:**

```python
import sys
sys.path.insert(0, os.path.expanduser('~/.nuke/nuke_tools_manager'))
import tool_manager_ui
tool_manager_ui.show()
```

**From Nuke's menubar** (after `menu.py` is in `~/.nuke/`):

```
Nuke  →  Tools Manager  →  Open Tools Manager
```

**Standalone (outside Nuke, for testing):**

```bash
cd ~/.nuke/nuke_tools_manager
python tool_manager_ui.py
```

---

## Tabs

### 📦 Install
- Browse to a `.zip` file
- Preview its contents before installing
- Choose destination (default: `~/.nuke/tools/<name>/`)
- Optionally auto-register the path with Nuke

### 🗜 Zip / Export
- Select a tools folder
- Filter by extension (`.py`, `.nk`, `.gizmo`, etc.)
- Quick presets: Python only, Nuke+Python, All common
- Creates a distributable `.zip`

### 🗂 Installed Tools
- Lists all tools recorded in `manifest.json`
- Expand rows to see individual files
- Open folder, register with Nuke, or uninstall

### 🎬 Nuke Utils
- **Node Finder** — search by name/class pattern and select
- **Batch Ops** — select all, deselect, disable/enable, delete, zoom to fit, copy
- **Knob Setter** — set any knob value across selected nodes
- **Script Info** — FPS, frame range, node count, top node classes
- **Node Creator** — create any node class by name

---

## zip_utils.py Public API

```python
from zip_utils import (
    zip_folder,           # Pack a folder → .zip
    list_zip_contents,    # Inspect a .zip without extracting
    install_from_zip,     # Extract + record in manifest
    uninstall_tool,       # Remove tool dir + manifest entry
    get_installed_tools,  # Returns full manifest dict
    register_tool_path,   # sys.path + nuke.pluginAddPath()
    register_all_installed,  # Register everything in manifest
)

# Example: zip and install in one go
zip_path = zip_folder('/my/tool_folder')
install_dir = install_from_zip(zip_path)
register_tool_path(install_dir)
```

---

## Manifest

After the first install, a `manifest.json` is created at `~/.nuke/tools/manifest.json`:

```json
{
  "my_tool": {
    "install_dir": "/Users/you/.nuke/tools/my_tool",
    "zip_source":  "/Downloads/my_tool.zip",
    "installed_at": "2025-06-01T12:34:56",
    "file_count": 12,
    "files": ["my_tool/__init__.py", "my_tool/core.py", ...]
  }
}
```

<img width="1033" height="1093" alt="image" src="https://github.com/user-attachments/assets/89d87c45-7feb-4909-bd8d-1fe65718ccf8" />

<img width="1032" height="1112" alt="image" src="https://github.com/user-attachments/assets/18696dc2-f6d1-4b16-bbd7-8c3a393e8917" />

<img width="1026" height="1105" alt="image" src="https://github.com/user-attachments/assets/0255d2af-4137-4f34-9106-390bb930f7a8" />

<img width="1029" height="1111" alt="image" src="https://github.com/user-attachments/assets/1bbec74a-69a1-4682-8d6e-3d9ac0867fd6" />

<img width="1019" height="1105" alt="image" src="https://github.com/user-attachments/assets/b491373e-bd63-4397-8ecc-e2c70d066d8b" />

<img width="1028" height="1082" alt="image" src="https://github.com/user-attachments/assets/8aceb352-f72e-4e4b-adf1-7e402a139a95" />

<img width="1014" height="1120" alt="image" src="https://github.com/user-attachments/assets/6571af74-5860-4840-a608-5f36c83b3405" />

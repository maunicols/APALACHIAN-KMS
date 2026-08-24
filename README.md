# MATAME - Appalachian

Survival horror game MVP. Survive one night in an Appalachian forest for 10 minutes.

## 📋 Overview

A psychological horror game where fear is the primary mechanic. Navigate the darkness, manage your fear, and survive until dawn.

**Current Status:** MVP Setup Phase  
**Target Release:** 2 weeks  
**Engine:** Godot 4.3 LTS  

---

## 🎮 Gameplay

- **Duration:** 10 minutes (one game cycle)
- **Objective:** Survive the night
- **Mechanics:**
  - Dynamic fear system (0-100%)
  - Day/night cycle
  - Enemy encounters (The Shadow)
  - Combat system (melee)
  - Campfire mechanic (safe zone)

### Controls (To be implemented)
- **WASD** - Move
- **E** - Interact/Pick up
- **Click** - Attack

---

## 🛠️ Development Setup

### Prerequisites
- **Godot 4.3 LTS** - Download from [godotengine.org](https://godotengine.org/download/windows)
- **Windows 10+** - For compilation
- **Git** - For version control

### Installation

```bash
# Clone repository
git clone https://github.com/maunicols/APALACHIAN-KMS.git
cd APALACHIAN-KMS

# Open in Godot
godot --path .
```

---

## 📦 Project Structure

```
APALACHIAN-KMS/
├── scenes/              # Godot scene files (.tscn)
│   └── main.tscn       # Main game scene (TODO)
│
├── scripts/             # GDScript files
│   ├── game_manager.gd  # Game orchestrator
│   ├── player.gd        # Player controller
│   ├── enemy.gd         # Enemy AI (Shadow)
│   └── fear_system.gd   # Fear mechanic
│
├── assets/
│   ├── audio/          # Sound files (.ogg)
│   │   ├── night_wind.ogg
│   │   ├── threat_sound.ogg
│   │   └── impact.ogg
│   └── sprites/        # 2D sprites (.png) - 10x10px
│       ├── player.png
│       └── shadow.png
│
├── build_scripts/
│   ├── build_windows.bat
│   └── build_windows.ps1
│
├── project.godot       # Engine configuration
├── export_presets.cfg  # Export settings
└── README.md           # This file
```

---

## 🎨 Visual Style

**Target Style:** 2D Minimalist

Specifications (provided by designer):
- **Color Palette:** Black, white, grays
- **Resolution:** Max 10x10 pixels per object
- **Character:** Humanoid oval (red legs)
- **Enemy:** Gray-bordered dark shapes
- **Tool (Palo):** Yellow base
- **Campfire:** Concentric black sticks + yellow flame (orange edges)

> Assets pending creative direction from designer

---

## 🚀 Build & Deploy

### Windows Compilation

**Option A: Batch Script (Recommended)**
```cmd
build_windows.bat
```

**Option B: PowerShell**
```powershell
powershell -ExecutionPolicy Bypass -File build_windows.ps1
```

**Option C: Command Line**
```cmd
godot --path . --export-release "Windows Desktop" export/windows/matame.exe
```

**Output:** `export/windows/matame.exe`

---

## 📊 Development Timeline

| Week | Deliverable |
|------|-------------|
| **Week 1** | Setup + Core scripts + Scene structure |
| **Week 2** | Assets + Polish + Build verification |

---

## 🎯 MVP Features

- [x] Project structure
- [x] Core scripts (game_manager, player, enemy, fear_system)
- [x] Build configuration
- [ ] Scene setup (main.tscn)
- [ ] Asset creation (sprites, audio)
- [ ] Game mechanics implementation
- [ ] Testing & debugging
- [ ] Release build

---

## 📝 Notes

- **Scripts are skeletal** - Core logic is commented and ready for expansion
- **Assets folder is empty** - Waiting for creative direction
- **CI/CD is deferred** - Will implement after MVP completion
- **No external dependencies** - Pure Godot 4.3

---

## 👤 Author

Developed by Claude (Anthropic)  
Based on game design by [User]

---

## 📄 License

MIT License - See LICENSE file for details

---

## 🔗 Resources

- [Godot Documentation](https://docs.godotengine.org/)
- [GDScript Reference](https://docs.godotengine.org/en/stable/tutorials/scripting/gdscript/index.html)
- [GitHub Repository](https://github.com/maunicols/APALACHIAN-KMS)

---

**Last Updated:** 2026-08-24  
**Status:** Setup Phase ✅

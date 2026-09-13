# CK3 Great Projects Fix (CK3 大型工程修复)

<div align="center">

[![Steam Workshop](https://img.shields.io/badge/Steam_Workshop-3799596844-blue.svg?logo=steam)](https://steamcommunity.com/sharedfiles/filedetails/?id=3799596844)
[![GitHub](https://img.shields.io/badge/GitHub-ck3--great--projects--fix-181717.svg?logo=github)](https://github.com/ShunbaoLi/ck3-great-projects-fix)
[![Changelog](https://img.shields.io/badge/Changelog-Keep_a_Changelog-blueviolet.svg)](CHANGELOG.md)
[![CK3 Version](https://img.shields.io/badge/CK3_Version-1.18%20%7C%201.19+-orange.svg)](https://ck3.paradoxwikis.com/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

**[简体中文](README.md) | [English](README.en.md)**

</div>

---

## 📌 About the Project
**CK3 Great Projects Fix** is a community fix mod dedicated to resolving logic deadlocks, AI loops, historical errors, and scope bugs across Crusader Kings III's Great Projects system.

Initially created for the Great Wall and Grand Canal in East Asia, this project has evolved into an **open, worldwide framework for repairing and enhancing all Great Projects across CK3**. Contributions and bug reports from players and modders are warmly welcome!

> 📜 **Complete Version History & Changes**: See [CHANGELOG.md](CHANGELOG.md).

---

## 🏰 Fixed Great Projects (Project Status Matrix)

| Project | In-Game ID | Region / Culture | Key Fixes | Documentation |
| :--- | :--- | :--- | :--- | :---: |
| **The Great Wall** (万里长城) | `great_wall` | Frontier China | Fixed AI initiation deadlock, 35 section scope failures under bureaucracy, Master Builder lock, invalid owner scopes | [📖 View Details](docs/great_wall.en.md) |
| **The Grand Canal** (隋唐大运河) | `grand_canals` | Central Plain / Jiangnan | Replaced straight post-Yuan alignment with authentic Tang-Song curved network through Luoyang & Kaifeng (23 counties) | [📖 View Details](docs/grand_canals.en.md) |

---

## 🤝 Community & Contributions Welcome

**This project welcomes bug reports and fixes for ALL Great Projects worldwide!**

If you encounter any logic deadlocks, progression bugs, AI quirks, or historical inaccuracies in any vanilla or DLC Great Projects (such as the Pyramids, Hagia Sophia, Colosseum, Angkor Wat, Grand Canals, Great Wall, etc.):

- **Report an Issue**: Open a ticket at [GitHub Issues](https://github.com/ShunbaoLi/ck3-great-projects-fix/issues) with reproduction details and savegame/screenshots.
- **Submit a Pull Request**:
  - Fork the repository and create a feature branch.
  - Note: Ensure all `.txt` and `.yml` files are encoded in **UTF-8 with BOM** (mandatory for the Clausewitz engine).
  - When submitting a PR, please append a brief note under the `[Unreleased]` section in [CHANGELOG.md](CHANGELOG.md).
  - Submit your [Pull Request](https://github.com/ShunbaoLi/ck3-great-projects-fix/pulls) for review!

---

## ✨ Compatibility & Specifications
- **Game Version**: Crusader Kings III 1.18+ / 1.19+ (compatible with Roads to Power / The Golden Peacock / TGP DLC).
- **Achievements**: Alters `common/` and `map_data/`, which changes the game checksum. Incompatible with ironman achievements.
- **Save Game Compatibility**: Fully save-game compatible. Can be safely enabled mid-campaign.
- **Modified Files**:
  - `common/great_projects/types/00_great_project_types.txt`
  - `map_data/geographical_regions/geographical_region.txt`
  - `localization/`

---

## 🛠️ Installation
- **Option A (Recommended): Steam Workshop**
  - Subscribe via the [Steam Workshop Page (ID: 3799596844)](https://steamcommunity.com/sharedfiles/filedetails/?id=3799596844).
- **Option B: Manual Installation**
  - Download or clone this repository into your CK3 user mod directory:
    - Windows: `%USERPROFILE%\Documents\Paradox Interactive\Crusader Kings III\mod\`
    - Linux: `~/.local/share/Paradox Interactive/Crusader Kings III/mod/`
    - macOS: `~/Documents/Paradox Interactive/Crusader Kings III/mod/`
- Enable **"CK3 大型工程修复 (CK3 Great Projects Fix)"** in your Paradox Launcher Playset.

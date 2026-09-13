# CK3 大型工程修复 (CK3 Great Projects Fix)

<div align="center">

[![Steam Workshop](https://img.shields.io/badge/Steam_Workshop-3799596844-blue.svg?logo=steam)](https://steamcommunity.com/sharedfiles/filedetails/?id=3799596844)
[![GitHub](https://img.shields.io/badge/GitHub-ck3--great--projects--fix-181717.svg?logo=github)](https://github.com/ShunbaoLi/ck3-great-projects-fix)
[![CK3 Version](https://img.shields.io/badge/CK3_Version-1.18%20%7C%201.19+-orange.svg)](https://ck3.paradoxwikis.com/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

**[中文说明](#-中文说明) | [English Description](#-english-description)**

</div>

---

## 🇨🇳 中文说明

### 📌 项目简介
**CK3 大型工程修复 (CK3 Great Projects Fix)** 致力于系统性修复《十字军之王3》（Crusader Kings III）中各大“大型工程”（Great Projects）存在的**底层逻辑缺陷、AI 死锁、历史穿帮、作用域判定错误与界面卡顿**问题。

本项目最初为了修正中华天朝区域的两大超级工程——“隋唐大运河”与“万里长城”而诞生。现已升级并扩展为**面向 CK3 全球所有大型工程的开源修复与改进框架**。我们热烈欢迎广大玩家与 Mod 作者共同参与发掘 Bug 并贡献修复方案！

---

### 🏰 当前已包含的修复模块

#### 模块一：万里长城修复 (The Great Wall)
针对原版“加固万里长城”（`great_wall`）中存在的严重脚本逻辑设计缺陷与卡顿进行了深度重构：
1. **AI 发起死锁修复**：
   - *原版问题*：原版判定仅检查“处于和平”且“未规划长城”，未校验领内是否真正拥有未满级（1–3级）长城段落。导致关中/京兆府等内阁要员频繁发起长城工程，发起后却无地可修，工程陷入永久死锁。
   - *修复方案*：在发起条件 (`can_start_planning`) 与 AI 意愿 (`ai_will_do`) 中增加前置校验，要求顶级领主领内必须存在等级 1–3 级且尚未升至满级（4级）的长城段落。无段落可修时 AI 发起意愿直接归零。
2. **35 处长城段落归属判定修复**：
   - *原版问题*：原版 35 个可选长城段落（`walls_01` 至 `walls_35`）仅使用 `province_owner ?= { top_liege = root.top_liege }`。在天朝官僚制或未分封男爵领下，`province_owner` 为空（由伯爵直领），导致界面上所有长城段落全部显示为“无法贡献”或直接消失。
   - *修复方案*：为全部 35 处长城段落提供严密的作用域回退机制（男爵领主 -> 伯爵领主 -> 帝国领地），确保官僚制、直辖或分封状态下均能正常投资建设。
3. **主建筑师死锁解除 (Master Builder Fix)**：
   - *原版问题*：主建筑师任命硬性要求必须已有地块获得资助。玩家或 AI 若优先选择委任主建筑师，按钮将永久锁死。
   - *修复方案*：只要领内存在可升级段落，随时允许资助主建筑师。
4. **朝廷官员发起有效性保护 (is_valid)**：
   - *原版问题*：有效性检查原版限定为 `scope:owner = { any_realm_county = { ... } }`。工部尚书等官员封地在京城而非塞北边境，发起瞬间即被系统误判为失效而作废。
   - *修复方案*：统一校验 `scope:owner.top_liege` 领内，确保朝廷官僚发起长城工程时稳健推进。
5. **界面点击秒开性能优化 (Performance Optimization)**：
   - *原版问题*：点击地图图钉展开工程面板时，原版脚本在界面每帧对每个男爵领反复执行全图层级的 `any_realm_county` 暴力遍历，在大帝国（如上百伯爵领的天朝）中引发长达数秒的画面严重卡死。
   - *修复方案*：将昂贵的 O(N) 遍历彻底重构为极速 O(1) 作用域回溯指针校验，彻底消除卡顿，实现毫秒级秒开。

#### 模块二：隋唐大运河路线与区域修正 (The Sui-Tang Grand Canal)
纠正原版大运河直接套用**元明清时期京杭直线大运河**（绕过洛阳与开封，错走鲁西曹州、单州、濮州）的历史穿帮：
- **历史真实水网还原**：去直取弯，彻底确立**东都洛阳**与**东京开封（汴梁）**的大运河核心枢纽地位。
- **四大渠段伯爵领修正**：
  - **江南运河**（浙东与江南段）：明州、越州、杭州、秀州、苏州。
  - **山阳渎**（淮扬与渡江段）：常州、润州、扬州、楚州、泗州。
  - **通济渠（汴河段）**：宿州、宋州（商丘）、**汴州（开封）**、郑州、**河南府（洛阳）**（彻底剔除原版无关的徐、单、曹、濮）。
  - **永济渠（御河段）**：**怀州（河阳）、卫州（汲县）、相州（安阳）**、魏州、贝州、德州、沧州、幽州（补全洛北沁水引水渠段）。

---

### 🤝 欢迎社区反馈与代码贡献 (Contributions Welcome)

**本项目不局限于天朝，欢迎扩展至全球各地的所有大型工程！**

如果你在游玩 CK3 时发现了任何大型工程（如埃及金字塔、君士坦丁堡圣索菲亚大教堂、罗马斗兽场、吴哥窟、大运河、长城等）存在的：
- 逻辑死锁 / 无法推进 / 无法完成
- 触发条件或发起资格判定 Bug
- AI 异常行为（无脑发起、空耗国库等）
- 界面卡顿或性能瓶颈
- 历史地理考据硬伤

非常欢迎通过以下方式参与：
1. **提交 Issue**：在 [GitHub Issues](https://github.com/ShunbaoLi/ck3-great-projects-fix/issues) 描述你发现的 Bug、重现步骤与存档截图。
2. **提交 Pull Request**：
   - Fork 本仓库并在本地进行修改。
   - 确保修改的文本文件（`.txt` / `.yml`）保持 **UTF-8 with BOM** 编码（CK3 引擎硬性要求）。
   - 提交 PR 并详细说明修复原理，我们会迅速审核并合并！

---

### ✨ 兼容性与技术特性
- **游戏版本**：Crusader Kings III 1.18+ / 1.19+（适配带有 Great Projects 机制的版本与《东亚与帝国》TGP DLC）。
- **成就兼容**：由于修改了 `common/` 与 `map_data/`，会变更 Checksum，不支持铁人成就。
- **存档兼容**：完美支持中途加入新开或已有存档；若旧存档中长城已处于无法贡献状态，加载本 Mod 后将立即解除限制。
- **文件覆盖范围**：
  - `common/great_projects/types/00_great_project_types.txt`
  - `map_data/geographical_regions/geographical_region.txt`
  - `localization/`

---

### 🛠️ 安装与启用
- **方式 A（推荐）：Steam 创意工坊订阅**
  - 访问 [Steam 创意工坊页面](https://steamcommunity.com/sharedfiles/filedetails/?id=3799596844) 点击“订阅”即可自动下载与更新。
- **方式 B：手动本地安装**
  - 将本仓库克隆或解压至你的 CK3 用户 Mod 目录：
    - Windows: `%USERPROFILE%\Documents\Paradox Interactive\Crusader Kings III\mod\`
    - Linux: `~/.local/share/Paradox Interactive/Crusader Kings III/mod/`
    - macOS: `~/Documents/Paradox Interactive/Crusader Kings III/mod/`
- 在 CK3 官方启动器的“播放集”（Playset）中勾选 **“CK3 大型工程修复 (CK3 Great Projects Fix)”** 即可。

---
---

## 🌐 English Description

### 📌 About the Project
**CK3 Great Projects Fix** is a comprehensive community fix mod dedicated to resolving **logic deadlocks, AI loops, historical errors, scope bugs, and UI freezes** in Crusader Kings III's Great Projects system.

Initially created to fix the game-breaking issues with the Great Wall and the Grand Canal in East Asia, this project has evolved into an **open, worldwide framework for repairing and enhancing all Great Projects across CK3**. Players and modders worldwide are warmly invited to discover bugs, report issues, and contribute fixes!

---

### 🏰 Currently Included Modules

#### Module 1: The Great Wall Fix (`great_wall`)
A complete overhaul of the vanilla "Reinforce the Great Wall" project to fix broken script logic and performance bottlenecks:
1. **AI Deadlock Prevention**:
   - *Vanilla Issue*: Vanilla only checks if the realm is at peace and has no active Great Wall project, failing to verify whether any unmaxed (Tiers 1–3) wall sections actually exist. AI ministers in Chang'an/Guanzhong repeatedly start hollow projects with zero valid sections to fund, permanently deadlocking the project.
   - *Our Fix*: Added robust conditions to `can_start_planning` and `ai_will_do`, ensuring the top liege holds at least one unmaxed section. AI initiative drops to 0 if all sections are fully completed or absent.
2. **Scope Repair for All 35 Wall Sections**:
   - *Vanilla Issue*: All 35 wall sections previously checked `province_owner ?= { top_liege = root.top_liege }`. Under imperial bureaucracy or ungranted baronies, `province_owner` evaluates to none (held directly by the count), causing all sections to disappear or display as "cannot contribute".
   - *Our Fix*: Implemented a 3-tier scope fallback (barony owner -> county holder -> realm territory) to ensure sections are always fundable regardless of feudalization or bureaucratic tenure.
3. **Master Builder Deadlock Resolved**:
   - *Vanilla Issue*: Appointment of the Master Builder strictly required `exists = scope:great_project.var:any_funded_provinces`. Choosing the Master Builder first permanently locked the option.
   - *Our Fix*: Allowed appointing the Master Builder at any time as long as upgradeable wall sections exist in the realm.
4. **Courtier / Minister Project Validity (`is_valid`)**:
   - *Vanilla Issue*: Vanilla checked `scope:owner = { any_realm_county = { ... } }`. Imperial chancellors or ministers holding fiefs near the capital were immediately aborted by the system upon initiation.
   - *Our Fix*: Scoped checks to `scope:owner.top_liege`, guaranteeing stability when imperial officials launch public works.
5. **UI Freeze & Lag Elimination (Performance Optimization)**:
   - *Vanilla Issue*: Opening the Great Wall map pin caused heavy frame drops and several seconds of freezing because the vanilla script repeatedly executed full-realm county iterations (O(N)) per barony per frame.
   - *Our Fix*: Optimized the check to an ultra-fast O(1) scope pointer lookup, completely eliminating UI lag and ensuring instant response even in massive empires.

#### Module 2: The Sui-Tang Grand Canal Route Fix (`grand_canals`)
Fixes the historical anachronism where the canal was modeled as the post-Yuan/Ming straight Beijing-Hangzhou canal (bypassing Luoyang and Kaifeng, mistakenly routing through Caozhou and Shan):
- **Faithful Tang-Song Waterway Network**: Restored the iconic curve with **Luoyang (Eastern Capital)** and **Kaifeng (Bianjing)** as the central transport hubs.
- **Counties Across Four Canal Sections**:
  - **Jiangnan Canal**: Mingzhou (Ningbo), Yuezhou (Shaoxing), Hangzhou, Xiuzhou (Jiaxing), Suzhou.
  - **Shanyang Channel**: Changzhou, Runzhou (Zhenjiang), Yangzhou, Chuzhou (Huai'an), Sizhou.
  - **Tongji Canal (Bian River)**: Suzhou (Anhui), Songzhou (Shangqiu), **Bianzhou (Kaifeng)**, Zhengzhou, **Henan Fu (Luoyang)**.
  - **Yongji Canal (Yu River)**: **Huaizhou (Heyang), Weizhou, Xiangzhou (Anyang)**, Weizhou (Hebei), Beizhou, Dezhou, Cangzhou, Youzhou (Beijing).

---

### 🤝 Community & Contributions Welcome

**This project welcomes bug reports and fixes for ALL Great Projects worldwide!**

If you encounter any logic deadlocks, progression bugs, AI quirks, UI freezes, or historical inaccuracies in any vanilla or DLC Great Projects (such as the Pyramids, Hagia Sophia, Colosseum, Angkor Wat, Grand Canals, Great Wall, etc.):

- **Report an Issue**: Open a ticket at [GitHub Issues](https://github.com/ShunbaoLi/ck3-great-projects-fix/issues) with reproduction details and savegame/screenshots.
- **Submit a Pull Request**:
  - Fork the repository and create a feature branch.
  - Note: Ensure all `.txt` and `.yml` files are encoded in **UTF-8 with BOM** (mandatory for the Clausewitz engine).
  - Submit your [Pull Request](https://github.com/ShunbaoLi/ck3-great-projects-fix/pulls) for review!

---

### ✨ Compatibility & Specifications
- **Game Version**: Crusader Kings III 1.18+ / 1.19+ (compatible with Roads to Power / The Golden Peacock / TGP DLC).
- **Achievements**: Alters `common/` and `map_data/`, which changes the game checksum. Incompatible with ironman achievements.
- **Save Game Compatibility**: Fully save-game compatible. Can be safely enabled mid-campaign.
- **Modified Files**:
  - `common/great_projects/types/00_great_project_types.txt`
  - `map_data/geographical_regions/geographical_region.txt`
  - `localization/`

---

### 🛠️ Installation
- **Option A (Recommended): Steam Workshop**
  - Subscribe via the [Steam Workshop Page (ID: 3799596844)](https://steamcommunity.com/sharedfiles/filedetails/?id=3799596844).
- **Option B: Manual Installation**
  - Download or clone this repository into your CK3 user mod directory:
    - Windows: `%USERPROFILE%\Documents\Paradox Interactive\Crusader Kings III\mod\`
    - Linux: `~/.local/share/Paradox Interactive/Crusader Kings III/mod/`
    - macOS: `~/Documents/Paradox Interactive/Crusader Kings III/mod/`
- Enable **"CK3 大型工程修复 (CK3 Great Projects Fix)"** in your Paradox Launcher Playset.

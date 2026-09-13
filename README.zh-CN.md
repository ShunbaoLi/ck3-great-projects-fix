# CK3 大型工程修复 (CK3 Great Projects Fix)

<div align="center">

[![Steam Workshop](https://img.shields.io/badge/Steam_Workshop-3799596844-blue.svg?logo=steam)](https://steamcommunity.com/sharedfiles/filedetails/?id=3799596844)
[![GitHub](https://img.shields.io/badge/GitHub-ck3--great--projects--fix-181717.svg?logo=github)](https://github.com/ShunbaoLi/ck3-great-projects-fix)
[![Changelog](https://img.shields.io/badge/Changelog-Keep_a_Changelog-blueviolet.svg)](CHANGELOG.md)
[![CK3 Version](https://img.shields.io/badge/CK3_Version-1.18%20%7C%201.19+-orange.svg)](https://ck3.paradoxwikis.com/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

**[English](README.md) | [简体中文](README.zh-CN.md)**

</div>

---

## 📌 项目简介
**CK3 大型工程修复 (CK3 Great Projects Fix)** 旨在系统性修复《十字军之王3》（Crusader Kings III）中各“大型工程”（Great Projects）系统存在的逻辑缺陷、AI 死锁、历史穿帮、作用域错误等 Bug。

本项目起步于针对“万里长城”与“隋唐大运河”的专项修复，现已升级为**面向 CK3 全球所有大型工程的通用修复框架**。欢迎社区玩家与开发者共同发掘 Bug 并贡献修复。

> 📜 **完整版本变动与历史记录**：请参阅 [CHANGELOG.md](CHANGELOG.md)。

---

## 🏰 当前已修复模块

### 模块一：万里长城修复 (`great_wall`)
1. **AI 发起死锁 Bug (AI Deadlock on Initiation)**：
   - 修复领内无可升级段落（1–3级）时，AI 官员仍频繁发起长城工程导致工程永久死锁且无法推进的 Bug。
2. **段落无法资助 Bug (Barony Contribution Scope Failure)**：
   - 修复在天朝官僚制或未分封男爵领下，全部 35 处长城段落因 `province_owner` 作用域无法解析而显示为“无法贡献”或直接在面板消失的 Bug。
3. **主建筑师任命死锁 Bug (Master Builder Deadlock)**：
   - 修复玩家或 AI 先行委任主建筑师时，因前置资助判定缺陷导致该选项被永久锁死的 Bug。
4. **朝廷官员发起工程即时失效 Bug (Invalid Project Owner Scope)**：
   - 修复中央内阁要员（如工部尚书）发起工程时，因直属封地不在边境导致工程被系统判定为无效而立刻作废的 Bug。

### 模块二：隋唐大运河路线修正 (`grand_canals`)
修复原版大运河区域错套用元明清京杭直线大运河的历史穿帮 Bug，去直取弯，恢复以东都洛阳与东京开封（汴梁）为中枢的唐宋水网：
- **江南运河**：明州、越州、杭州、秀州、苏州。
- **山阳渎**：常州、润州、扬州、楚州、泗州。
- **通济渠（汴河段）**：宿州、宋州（商丘）、**汴州（开封）**、郑州、**河南府（洛阳）**（彻底剔除原版无关的徐、单、曹、濮）。
- **永济渠（御河段）**：**怀州（河阳）、卫州（汲县）、相州（安阳）**、魏州、贝州、德州、沧州、幽州（补全洛北沁水引水渠段）。

---

## 🤝 欢迎社区反馈与代码贡献 (Contributions Welcome)

**本项目不局限于天朝，欢迎扩展至全球各地的所有大型工程！**

如果你在游玩 CK3 时发现了任何大型工程（如金字塔、圣索菲亚大教堂、罗马斗兽场、吴哥窟、大运河、长城等）存在的：
- 逻辑死锁 / 无法推进 / 无法完成
- 触发条件或发起资格判定 Bug
- AI 异常行为（无脑发起、空耗国库等）
- 历史地理考据硬伤

欢迎通过以下方式参与：
1. **提交 Issue**：在 [GitHub Issues](https://github.com/ShunbaoLi/ck3-great-projects-fix/issues) 描述 Bug 现象、重现步骤与存档截图。
2. **提交 Pull Request**：
   - Fork 本仓库并进行修复。
   - 确保修改的文本文件（`.txt` / `.yml`）保持 **UTF-8 with BOM** 编码（CK3 引擎硬性要求）。
   - 提交 PR 时，请同步在 [CHANGELOG.md](CHANGELOG.md) 顶部的 `[Unreleased]` 区块追加简短说明。

---

## ✨ 兼容性与技术特性
- **游戏版本**：Crusader Kings III 1.18+ / 1.19+（适配带有 Great Projects 机制的版本与《东亚与帝国》TGP DLC）。
- **成就兼容**：因修改了 `common/` 与 `map_data/`，会变更 Checksum，不支持铁人成就。
- **存档兼容**：完美支持中途加入新开或已有存档；若旧存档中长城已处于无法贡献状态，加载本 Mod 后将立即解除限制。
- **文件覆盖范围**：
  - `common/great_projects/types/00_great_project_types.txt`
  - `map_data/geographical_regions/geographical_region.txt`
  - `localization/`

---

## 🛠️ 安装与启用
- **方式 A（推荐）：Steam 创意工坊订阅**
  - 访问 [Steam 创意工坊页面](https://steamcommunity.com/sharedfiles/filedetails/?id=3799596844) 点击“订阅”即可自动下载与更新。
- **方式 B：手动本地安装**
  - 将本仓库克隆或解压至你的 CK3 用户 Mod 目录：
    - Windows: `%USERPROFILE%\Documents\Paradox Interactive\Crusader Kings III\mod\`
    - Linux: `~/.local/share/Paradox Interactive/Crusader Kings III/mod/`
    - macOS: `~/Documents/Paradox Interactive/Crusader Kings III/mod/`
- 在 CK3 官方启动器的“播放集”（Playset）中勾选 **“CK3 大型工程修复 (CK3 Great Projects Fix)”** 即可。

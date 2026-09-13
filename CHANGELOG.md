# 更新日志 (Changelog)

All notable changes to this project will be documented in this file.  
本项目的所有重要更新均记录于此文件。

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [Unreleased] - 待发布
<!-- 社区贡献者请将新修复直接追加到下方对应分类中 / Contributors, please append changes below -->

### Added
### Fixed
### Changed

---

## [1.2.0] - 2026-09-13
### Changed
- **项目品牌全面升级 (Project Rebrand)**：
  - [CN] Mod 正式更名为 **“CK3 大型工程修复 (CK3 Great Projects Fix)”**，维护范围扩展为全球所有大型工程。
  - [EN] Rebranded to **"CK3 Great Projects Fix"**, expanding scope to all Great Projects across CK3 worldwide.
- **文档体系标准化 (Documentation)**：
  - [CN] 重构双语说明文档与规范化的 `CHANGELOG.md`，建立社区开源协作指南。
  - [EN] Implemented standardized bilingual documentation, `CHANGELOG.md`, and community contribution guides.
- **隐私与本地化清理 (Sanitization)**：
  - [CN] 彻底清理所有开发环境本地盘符路径暴露，替换为跨平台标准安装指引。
  - [EN] Removed all local development path references in favor of standard multi-platform installation instructions.

---

## [1.1.0] - 2026-09-12
### Added
- **万里长城大型工程修复 (The Great Wall Overhaul)**：
  - [CN] 合并万里长城大型工程修复模块（`great_wall`）。
  - [EN] Integrated The Great Wall overhaul module (`great_wall`).

### Fixed
- **万里长城 (`great_wall`)**：
  - [CN] **AI 发起死锁**：增加领内存在 1–3 级可升级段落的前置校验，修复当长城已全部满级或无段落可修时，AI 角色仍盲目发起空工程导致工程永久死锁的问题。
  - [EN] **AI Deadlock on Initiation**: Enforced requirement for unmaxed (Tiers 1–3) wall sections in the realm, preventing AI characters from repeatedly launching deadlocked empty projects when all sections are maxed.
  - [CN] **35 处段落归属**：为全部 35 处段落增加作用域回退机制，修复天朝官僚制与未分封男爵领下所有段落无法资助或在面板消失的 Bug。
  - [EN] **Barony Contribution Scope Failure**: Added robust 3-tier scope fallbacks for all 35 wall sections, fixing contribution unavailability under imperial bureaucracy or ungranted baronies.
  - [CN] **主建筑师死锁**：解除必须已有地块受资助的硬性前置限制，允许在任意时间委任主建筑师。
  - [EN] **Master Builder Deadlock**: Removed circular requirement for pre-funded provinces before appointing the Master Builder.
  - [CN] **官员发起工程失效**：修正有效性校验范围为帝国顶级领主，防止工部尚书或中书省摄政因直属封地不在边境导致工程被系统直接作废。
  - [EN] **Premature Invalidation**: Scoped validity checks to top liege realm, preventing premature invalidation when ministers (e.g. Minister of Works or Grand Secretariat Regents) hold demesne outside border areas.

---

## [1.0.0] - 2026-09-10
### Added
- **隋唐大运河路线修正 (Sui-Tang Grand Canal Route Fix)**：
  - [CN] 纠正原版错用明清直线京杭大运河的历史穿帮。
  - [EN] Corrected vanilla anachronism modeling the post-Yuan straight canal instead of the Tang-Song network.
  - [CN] 恢复中原唐宋真实水运漕网，重新确立东都洛阳与东京开封（汴梁）的大运河中枢地位。
  - [EN] Restored Luoyang and Kaifeng as central hubs of the imperial canal waterway network.
  - [CN] 修正江南运河、山阳渎、通济渠与永济渠 4 大渠段共 23 个伯爵领地理区域归属。
  - [EN] Corrected county composition across Jiangnan, Shanyang, Tongji, and Yongji canal sections.

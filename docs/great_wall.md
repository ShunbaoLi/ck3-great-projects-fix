# 🏰 万里长城大型工程修复文档 (The Great Wall)

**[中文](great_wall.md) | [English](great_wall.en.md) | [返回主页 / Back to Home](../README.md)**

---

## 📌 工程背景
“加固万里长城”（`great_wall`）是《十字军之王3》（CK3）在 1.18 版本与《东亚与帝国》（TGP）DLC 中引入的大型工程。在原版脚本（`common/great_projects/types/00_great_project_types.txt`）中，存在多处严重的底层逻辑设计缺陷，导致天朝剧本体验受到极大破坏。

---

## 🔍 原版 Bug 与修复方案详解

### 1. AI 发起死锁 Bug (AI Deadlock on Initiation)
- **原版问题**：
  原版的发起条件（`can_start_planning`）仅检查了“处于和平状态”与“领内当前未在规划长城”，但**完全未校验领内是否真正拥有未满级（1–3级）的长城段落**。
  这导致京兆府留后、工部尚书等朝廷官员在长安频繁发起长城工程，但领内根本没有可修段落，导致工程永久卡死在准备阶段、无法推进也无法取消。
- **修复方案**：
  在发起条件 (`can_start_planning`) 与 AI 发起意愿 (`ai_will_do`) 中增加前置逻辑校验，硬性要求顶级领主（皇帝）领内必须存在等级为 1–3 级且尚未升至满级（4级）的长城段落。无段落可修时，AI 发起意愿直接降为 0。

---

### 2. 35 处长城段落“无法资助”Bug (Contribution Scope Failure)
- **原版问题**：
  全部 35 个可选长城段落（`walls_01` 至 `walls_35`）的展示与资助条件在原版中写为：
  ```pdx
  province_owner ?= { top_liege = root.top_liege }
  ```
  在天朝官僚制（Bureaucracy）或男爵领未单独分封的情况下，`province_owner` 为空（该男爵领由伯爵直辖），导致界面上所有长城段落均显示为“无法贡献”，或直接在可选列表中完全消失。
- **修复方案**：
  为全部 35 处长城段落增加了完备的作用域回退校验机制：
  ```pdx
  OR = {
      province_owner ?= { top_liege = scope:owner.top_liege }
      county.holder ?= { top_liege = scope:owner.top_liege }
      county.holder.top_liege = scope:owner.top_liege
  }
  ```
  保证无论长城段落是由独立男爵管理、伯爵兼领，还是官僚节度使管辖，只要处于帝国疆域内，均能正常显示并允许投资建设。

---

### 3. 主建筑师前置死锁 Bug (Master Builder Deadlock)
- **原版问题**：
  主建筑师任命条件硬性要求：
  ```pdx
  exists = scope:great_project.var:any_funded_provinces
  ```
  即必须先有至少一个地块获得资助。如果玩家或 AI 优先点击了委任主建筑师，按钮将永久锁死，形成死锁循环。
- **修复方案**：
  只要领内存在可升级段落，即允许直接资助并委任主建筑师，解除循环前置依赖。

---

### 4. 朝廷官员发起工程即时作废 Bug (Invalid Project Owner Scope)
- **原版问题**：
  工程有效性检查（`is_valid`）原版写为：
  ```pdx
  scope:owner = { any_realm_county = { ... } }
  ```
  当工部尚书或中央内阁官员发起长城工程时，其自身直属封地往往在长安、洛阳等腹地，其直属领地内并不包含塞北长城，导致工程发起瞬间即被系统判定为无效而立刻作废。
- **修复方案**：
  统一将有效性作用域提升为校验 `scope:owner.top_liege` 领内，确保朝廷官僚发起长城工程时稳健推进。

---

## ⚡ 性能优化说明 (Technical Notes)
原版界面在渲染地图图钉列表时，对每个男爵领反复执行全图层级遍历，在大帝国（拥有数百伯爵领）下会导致明显的交互卡顿。本 Mod 将其重构为轻量的直接指针比对（$O(1)$ 指针作用域查找），彻底保证了长城工程界面的毫秒级响应。

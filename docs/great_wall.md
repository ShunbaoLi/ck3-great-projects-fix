# 🏰 万里长城大型工程修复文档 (The Great Wall)

**[中文](great_wall.md) | [English](great_wall.en.md) | [返回主页 / Back to Home](../README.md)**

---

## 📌 工程背景
“加固万里长城”（`great_wall`）是《十字军之王3》（CK3）在 1.18 版本与《东亚与帝国》（TGP）DLC 中引入的大型工程。在原版脚本文件 `common/great_projects/types/00_great_project_types.txt` 中，该工程限定仅能由以下三类角色发起：
1. **天朝皇帝 / 霸权持有者**（`has_title = title:h_china`）
2. **工部尚书**（`has_title = title:e_minister_of_works`）
3. **中书省摄政 / 辅政大臣**（二元统治类型为 `grand_secretariat` 的 `diarch`）

原版脚本在触发逻辑、作用域判定和有效性校验上存在多处底层缺陷，导致严重的逻辑死锁与工程异常。

---

## 🔍 原版 Bug 与修复方案详解

### 1. AI 发起死锁 Bug (AI Deadlock on Initiation)
- **原版代码缺陷**：
  原版的发起条件（`can_start_planning`）与 AI 发起意愿（`ai_will_do`）仅校验了“处于和平状态”与“当前未在规划长城”，**完全未校验帝国领内是否真正拥有未升至满级的长城段落（1–3级）**。
  当帝国境内全部 35 处长城均已升至 4 级满级，或领内已无可升级段落时，符合资格的 AI 角色（皇帝、工部尚书或中书省辅政大臣）依然会周期性触发 AI 逻辑发起长城工程。工程发起后没有任何地块可供资助推进，导致工程永久卡在准备阶段，且阻塞了后续工程的发起。
- **修复方案**：
  在 `can_start_planning` 与 `ai_will_do` 中增加严格的前置校验：
  ```pdx
  top_liege = {
      any_realm_county = {
          any_county_province = {
              has_building_or_higher = the_great_wall_01
              NOT = { has_building = the_great_wall_04 }
          }
      }
  }
  ```
  要求帝国境内必须存在等级为 1–3 级且尚未升至满级的长城。无可用升级段落时，发起条件直接关闭，AI 发起意愿降为 0。

---

### 2. 35 处长城段落“无法资助”Bug (Contribution Scope Failure)
- **原版代码缺陷**：
  全部 35 个可选长城段落（`walls_01` 至 `walls_35`）的展示与资助条件在原版中写为：
  ```pdx
  province_owner ?= { top_liege = root.top_liege }
  ```
  在天朝官僚制（Bureaucracy）或男爵领未单独分封的情况下，`province_owner` 为空（该男爵领由伯爵直领），导致界面上所有长城段落均显示为“无法贡献”，或直接在可选列表中完全消失。
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
- **原版代码缺陷**：
  主建筑师任命条件硬性要求：
  ```pdx
  exists = scope:great_project.var:any_funded_provinces
  ```
  即必须先有至少一个地块获得资助。如果玩家或 AI 优先点击了委任主建筑师，按钮将永久锁死，形成死锁循环。
- **修复方案**：
  只要领内存在可升级段落，即允许直接资助并委任主建筑师，解除循环前置依赖。

---

### 4. 朝廷官员发起工程即时作废 Bug (Invalid Project Owner Scope)
- **原版代码缺陷**：
  工程有效性检查（`is_valid`）原版写为：
  ```pdx
  scope:owner = { any_realm_county = { ... } }
  ```
  当工程由工部尚书（`title:e_minister_of_works`）或中书省辅政大臣（`diarch`）发起时，`scope:owner` 指向该官员个人而非皇帝。官员个人的直属领地（Realm）通常位于中原或京畿，其名下封地并不直接包含塞北边境长城伯爵领，导致工程在发起后的首个游戏 tick 判定 `is_valid = no`，被系统强制判定为无效而立刻作废。
- **修复方案**：
  将有效性作用域提升为校验顶级领主领内：
  ```pdx
  scope:owner.top_liege = { any_realm_county = { ... } }
  ```
  确保朝廷内阁官员代表朝廷发起长城工程时稳健推进。

---

## ⚡ 性能优化说明 (Technical Notes)
原版界面在渲染地图图钉列表时，对每个男爵领反复执行全图层级遍历，在大帝国下会导致明显的交互卡顿。本 Mod 将其重构为轻量的直接指针比对（$O(1)$ 指针作用域查找），保证了长城工程界面的毫秒级响应。

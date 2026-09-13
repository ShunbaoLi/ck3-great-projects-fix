# 🏰 The Great Wall Overhaul Documentation

**[中文](great_wall.md) | [English](great_wall.en.md) | [Back to Home](../README.md)**

---

## 📌 Background
"Reinforce the Great Wall" (`great_wall`) is a Great Project introduced in Crusader Kings III version 1.18 and the Roads to Power / The Golden Peacock (TGP) DLC. In vanilla CK3 scripts (`common/great_projects/types/00_great_project_types.txt`), several severe logical flaws break the gameplay loop for imperial China.

---

## 🔍 Detailed Vanilla Bugs & Fixes

### 1. AI Deadlock on Initiation
- **Vanilla Bug**:
  The vanilla planning trigger (`can_start_planning`) only checked whether the realm is at peace and whether another Great Wall project was already being planned. It **never validated if the realm actually contains any upgradeable (Tiers 1–3) wall sections**.
  This caused imperial ministers (e.g. the Minister of Works in Chang'an) to repeatedly launch hollow Great Wall projects with zero available sections to repair, permanently deadlocking the project.
- **Our Fix**:
  Added prerequisites in `can_start_planning` and `ai_will_do`, strictly requiring the top liege's realm to hold at least one wall section below tier 4. AI initiative drops to 0 if all sections are maxed or none exist.

---

### 2. Barony Contribution Scope Failure (Bureaucracy Bug)
- **Vanilla Bug**:
  All 35 wall contribution slots (`walls_01` through `walls_35`) used the following strict trigger:
  ```pdx
  province_owner ?= { top_liege = root.top_liege }
  ```
  Under imperial bureaucracy or whenever baronies are ungranted (held directly by the count), `province_owner` evaluates to none. Consequently, all 35 wall sections showed up as "cannot contribute" or vanished entirely from the UI.
- **Our Fix**:
  Implemented a robust 3-tier scope fallback:
  ```pdx
  OR = {
      province_owner ?= { top_liege = scope:owner.top_liege }
      county.holder ?= { top_liege = scope:owner.top_liege }
      county.holder.top_liege = scope:owner.top_liege
  }
  ```
  Whether a section is governed by an individual baron, directly by a count, or by a bureaucratic governor, it remains properly recognized and fundable.

---

### 3. Master Builder Deadlock
- **Vanilla Bug**:
  Appointing the Master Builder strictly required:
  ```pdx
  exists = scope:great_project.var:any_funded_provinces
  ```
  If a player or AI appointed the Master Builder first before funding a province, the option locked permanently, creating an unresolvable circular dependency.
- **Our Fix**:
  Removed the pre-funding restriction; appointing the Master Builder is permitted anytime upgradeable sections exist in the realm.

---

### 4. Premature Project Invalidation for Court Ministers
- **Vanilla Bug**:
  The project validity condition (`is_valid`) checked:
  ```pdx
  scope:owner = { any_realm_county = { ... } }
  ```
  When central ministers initiated the project, their personal demesne was centered around the imperial capital, not the frontier. The game engine immediately judged the project invalid and aborted it.
- **Our Fix**:
  Elevated the scope check to `scope:owner.top_liege`, ensuring stability when imperial bureaucrats manage public works.

---

## ⚡ Technical & Performance Notes
Vanilla scripts repeatedly executed full-realm iterations per barony per frame when rendering map pins. In large empires, this caused noticeable UI stuttering. This mod refactors the check into an instantaneous $O(1)$ scope pointer lookup, guaranteeing sub-millisecond UI responsiveness.

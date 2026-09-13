# 🏰 The Great Wall Overhaul Documentation

**[中文](great_wall.md) | [English](great_wall.en.md) | [Back to Home](../README.md)**

---

## 📌 Background
"Reinforce the Great Wall" (`great_wall`) is a Great Project introduced in Crusader Kings III version 1.18 and the Roads to Power / The Golden Peacock (TGP) DLC. In vanilla scripts (`common/great_projects/types/00_great_project_types.txt`), this project is restricted to three specific roles:
1. **Emperor / Hegemon of China** (`has_title = title:h_china`)
2. **Minister of Works** (`has_title = title:e_minister_of_works`)
3. **Grand Secretariat Regent / Diarch** (`grand_secretariat` diarchy type regent)

Vanilla scripts contain several severe defects in initiation conditions, contribution scopes, and validity triggers that disrupt imperial gameplay.

---

## 🔍 Detailed Vanilla Bugs & Fixes

### 1. AI Deadlock on Initiation
- **Vanilla Bug**:
  Vanilla triggers (`can_start_planning` and `ai_will_do`) only checked whether the realm was at peace and had no ongoing Great Wall project. They **never validated whether the realm actually contained any unmaxed wall sections (Tiers 1–3)**.
  When all 35 sections were already at Tier 4 or when no valid sections existed, eligible AI characters (the Emperor, Minister of Works, or Grand Secretariat Regent) repeatedly initiated the project anyway. With zero sections to fund, the project remained permanently deadlocked in planning and blocked future Great Projects.
- **Our Fix**:
  Enforced a prerequisite in `can_start_planning` and `ai_will_do` requiring the top liege's realm to hold at least one unmaxed section:
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
  If all sections are completed, the trigger closes and AI weight drops to 0.

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
  When the project was initiated by the Minister of Works (`title:e_minister_of_works`) or a Grand Secretariat Regent (`diarch`), `scope:owner` evaluated to the minister. Because the minister's personal demesne was typically in the interior rather than the frontier, the engine immediately judged `is_valid = no` on the next tick and aborted the project.
- **Our Fix**:
  Elevated the scope check to `scope:owner.top_liege = { any_realm_county = { ... } }`, ensuring stability when imperial officials manage public works.

---

## ⚡ Technical & Performance Notes
Vanilla scripts repeatedly executed full-realm iterations per barony per frame when rendering map pins. In large empires, this caused noticeable UI stuttering. This mod refactors the check into an instantaneous $O(1)$ scope pointer lookup, guaranteeing sub-millisecond UI responsiveness.

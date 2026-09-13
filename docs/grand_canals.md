# 🌊 隋唐大运河路线修正文档 (The Sui-Tang Grand Canal)

**[中文](grand_canals.md) | [English](grand_canals.en.md) | [返回主页 / Back to Home](../README.md)**

---

## 📌 修改范围
- **地理区域定义**：`map_data/geographical_regions/geographical_region.txt`
- **本地化文本**：`localization/simp_chinese/z_celestial_projects_l_simp_chinese.yml` 与 `localization/english/z_celestial_projects_l_english.yml`

---

## 🔍 原版缺陷 (Issue)
原版中“大运河”（`grand_canals`）相关的地理区域判定（`dlc_tgp_grand_canal_region`）错误采用了元明清时期的京杭直线运河走向：
1. **通济渠段缺失核心中枢**：原版 `dlc_tgp_grand_canal_3_region` 直接穿越鲁西四州，完全绕过了唐宋时期的中原漕运核心——河南府（东都洛阳）与汴州（东京开封）。
2. **永济渠段断头**：原版 `dlc_tgp_grand_canal_4_region` 缺少洛北黄河/沁水引水渠段，导致运河北段与中原水网脱节。

---

## 🛠️ 伯爵领改动明细 (County Changes)

### 1. `dlc_tgp_grand_canal_3_region`（通济渠 / 汴河段）
- **移出伯爵领**：`c_xuzhou`（徐州）、`c_danzhou`（单州）、`c_caozhou`（曹州）、`c_puzhou`（濮州）
- **新增伯爵领**：
  - `c_suzhou`（宿州）
  - `c_songzhou`（宋州 / 商丘）
  - `c_bianzhou`（汴州 / 开封）
  - `c_zhengzhou`（郑州）
  - `c_henan`（河南府 / 洛阳）

### 2. `dlc_tgp_grand_canal_4_region`（永济渠 / 御河段）
- **新增伯爵领**（补全引水段）：
  - `c_huaizhou`（怀州 / 河阳）
  - `c_weizhou_1`（卫州 / 汲县）
  - `c_xiangzhou`（相州 / 安阳）
- **保留伯爵领**：`c_weizhou`（魏州）、`c_beizhou`（贝州）、`c_dezhou`（德州）、`c_cangzhou`（沧州）、`c_youzhou`（幽州 / 北京）

### 3. `dlc_tgp_grand_canal_1_region`（江南运河段）
- **涵盖伯爵领**：`c_mingzhou_1`（明州）、`c_yuezhou`（越州）、`c_hangzhou`（杭州）、`c_xiuzhou`（秀州）、`c_suzhou_2`（苏州）

### 4. `dlc_tgp_grand_canal_2_region`（山阳渎 / 淮扬段）
- **涵盖伯爵领**：`c_changzhou`（常州）、`c_runzhou`（润州）、`c_yangzhou`（扬州）、`c_chuzhou_1`（楚州）、`c_sizhou_2`（泗州）

---

## 🏛️ 本地化覆盖 (Localization)
同步覆盖并修正以下本地化键值，使其与唐宋历史渠名及路线一致：
- `dlc_tgp_grand_canal_region`: "隋唐大运河"
- `dlc_tgp_grand_canal_1_region`: "江南运河"
- `dlc_tgp_grand_canal_2_region`: "山阳渎与淮扬段"
- `dlc_tgp_grand_canal_3_region`: "通济渠（汴河段）"
- `dlc_tgp_grand_canal_4_region`: "永济渠（御河段）"
- `great_project_type_grand_canals_desc`: 更新大运河历史描述文本

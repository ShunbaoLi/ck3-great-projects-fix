# 🌊 The Sui-Tang Grand Canal Route Fix Documentation

**[中文](grand_canals.md) | [English](grand_canals.en.md) | [Back to Home](../README.md)**

---

## 📌 Scope of Changes
- **Geographical Regions**: `map_data/geographical_regions/geographical_region.txt`
- **Localization Files**: `localization/simp_chinese/z_celestial_projects_l_simp_chinese.yml` and `localization/english/z_celestial_projects_l_english.yml`

---

## 🔍 Vanilla Defects (Issue)
In vanilla CK3, the geographical regions for the Grand Canal (`grand_canals`) erroneously modeled the post-Yuan/Ming straight canal route instead of the authentic medieval Tang-Song alignment:
1. **Missing Central Transport Hubs in Section 3**: `dlc_tgp_grand_canal_3_region` routed directly through western Shandong, completely bypassing the historical economic hubs of Luoyang (Eastern Capital) and Kaifeng (Bianjing).
2. **Broken Northern Intake in Section 4**: `dlc_tgp_grand_canal_4_region` omitted the canal intake counties along the Yellow/Qin River north of Luoyang, disconnecting the northern canal from the central network.

---

## 🛠️ County Changes (Diff by Region)

### 1. `dlc_tgp_grand_canal_3_region` (Tongji Canal / Bian River)
- **Removed Counties**: `c_xuzhou`, `c_danzhou`, `c_caozhou`, `c_puzhou`
- **Added Counties**:
  - `c_suzhou` (Suzhou / Anhui)
  - `c_songzhou` (Songzhou / Shangqiu)
  - `c_bianzhou` (Bianzhou / Kaifeng)
  - `c_zhengzhou` (Zhengzhou)
  - `c_henan` (Henan Fu / Luoyang)

### 2. `dlc_tgp_grand_canal_4_region` (Yongji Canal / Yu River)
- **Added Counties** (restoring intake section):
  - `c_huaizhou` (Huaizhou / Heyang)
  - `c_weizhou_1` (Weizhou / Jixian)
  - `c_xiangzhou` (Xiangzhou / Anyang)
- **Retained Counties**: `c_weizhou`, `c_beizhou`, `c_dezhou`, `c_cangzhou`, `c_youzhou`

### 3. `dlc_tgp_grand_canal_1_region` (Jiangnan Canal)
- **Counties**: `c_mingzhou_1`, `c_yuezhou`, `c_hangzhou`, `c_xiuzhou`, `c_suzhou_2`

### 4. `dlc_tgp_grand_canal_2_region` (Shanyang Channel)
- **Counties**: `c_changzhou`, `c_runzhou`, `c_yangzhou`, `c_chuzhou_1`, `c_sizhou_2`

---

## 🏛️ Localization Updates
Updated the following localization keys to match the historical channel names and descriptions:
- `dlc_tgp_grand_canal_region`: "Sui-Tang Grand Canal"
- `dlc_tgp_grand_canal_1_region`: "Jiangnan Canal"
- `dlc_tgp_grand_canal_2_region`: "Shanyang Channel and Huai-Yang Section"
- `dlc_tgp_grand_canal_3_region`: "Tongji Canal (Bian River Section)"
- `dlc_tgp_grand_canal_4_region`: "Yongji Canal (Yu River Section)"
- `great_project_type_grand_canals_desc`: Updated historical overview text

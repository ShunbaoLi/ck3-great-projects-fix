# 🌊 The Sui-Tang Grand Canal Route Fix Documentation

**[中文](grand_canals.md) | [English](grand_canals.en.md) | [Back to Home](../README.md)**

---

## 📌 Historical Context & Overview
In Crusader Kings III, the Grand Canal (`grand_canals`) represents China's vital inland waterways. However, vanilla CK3 mistakenly routed the canal based on the **post-Yuan/Ming dynasty straight Beijing-Hangzhou canal**:
- The vanilla route forced the canal straight through western Shandong counties (Caozhou, Shanzhou, Puzhou, and Xuzhou).
- **Severe Historical Anachronism**: In medieval Tang and Song China, the canal followed the Bian River upstream, with **Luoyang (Eastern Capital)** and **Kaifeng (Bianjing)** serving as the absolute economic and transport heart of the empire!

This overhaul replaces the straight post-medieval alignment with the authentic Sui-Tang curved waterway network centered on Luoyang and Kaifeng.

---

## 🗺️ County Breakdown Across Four Canal Sections

The 4 canal regions defined in `map_data/geographical_regions/geographical_region.txt` have been corrected as follows:

| Region ID | Historical Section | Included Counties | Historical Rationale |
| :--- | :--- | :--- | :--- |
| **`dlc_tgp_grand_canal_1_region`** | **Jiangnan Canal** | Mingzhou (Ningbo), Yuezhou (Shaoxing), Hangzhou, Xiuzhou (Jiaxing), Suzhou | The southern terminus (Hangzhou), connecting Lake Tai plain with eastern Zhejiang. |
| **`dlc_tgp_grand_canal_2_region`** | **Shanyang Channel** | Changzhou, Runzhou (Zhenjiang), Yangzhou, Chuzhou (Huai'an), Sizhou | Crosses the Yangtze from Zhenjiang to Yangzhou, following the ancient Shanyang channel to the Huai River at Sizhou. |
| **`dlc_tgp_grand_canal_3_region`** | **Tongji Canal (Bian River)** | **Suzhou (Anhui), Songzhou (Shangqiu), Bianzhou (Kaifeng), Zhengzhou, Henan Fu (Luoyang)** | **【Core Fix】** Stripped irrelevant western Shandong counties.<br>Follows the Bian River upstream through Kaifeng directly to **Luoyang**! |
| **`dlc_tgp_grand_canal_4_region`** | **Yongji Canal (Yu River)** | **Huaizhou (Heyang), Weizhou, Xiangzhou (Anyang)**, Weizhou (Hebei), Beizhou, Dezhou, Cangzhou, Youzhou (Beijing) | **【Core Fix】** Restored northern intake sections along the Qin River (Huaizhou, Weizhou, Xiangzhou) leading straight north to Youzhou. |

---

## 🏛️ Localization
Matching localization updates (in `localization/`) update in-game descriptions and names to reflect authentic Tang-Song historical context.

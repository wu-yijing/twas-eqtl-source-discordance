# figure_scripts_officialZ_20260917

**本目录是 2026-09-17 P0 修复后唯一的出图脚本来源。**

## 为什么需要它

原 `figure_scripts/` 中的脚本读取 `data/processed/*.csv`，而该目录产生于**实现缺陷修正之前**
（缺 σᵢ 表达方差因子 + PLINK 2-bit 解码错误），会系统性放大密模型基因的 |Z|：

| 基因 | 陈旧层值 | 官方 MetaXcan v0.8.1 值 |
|---|---|---|
| TUBB（eQTLGen, DR） | 48.52 | 11.89 |
| RNH1（eQTLGen, DR） | 13.32 | 2.31 |
| RNH1（GTEx Nerve_Tibial, DR） | 13.82 | 2.67 |
| CKAP4（eQTLGen, DR） | 4.75 | 0.99 |

后果：Fig. 3 / 4 / 5 / 6 / 7 / 8 的**图形**与稿件**正文数值**不一致——正文早已改用官方
MetaXcan v0.8.1 的 Z，图却没有跟着重出。本目录的脚本修复了这一点。

## 执行顺序

1. `00_build_officialZ_data_layer.py` — 从 Additional file 1 的官方表导出 `data/processed_officialZ/`
2. `01_redraw_Fig5_Fig7.py` — 候选基因 dumbbell + RNH1 跨队列
3. `02_redraw_Fig3.py` — 头条散点图（96 对）
4. `03_redraw_Fig6.py` — 富集三面板（按来源分离）
5. `04_redraw_Fig8.py` — 跨性状三基因集
6. `05_recompute_arms.py` — 核验 Table 3b 与 45 基因共同宇宙
7. `06_redraw_Fig4.py` — 轴分解 + Δρ 配对 bootstrap
8. `07_verify_SCZ_denominators.py` — 核验 SCZ 分母

## 数据纪律

所有脚本**只读** `data/processed_officialZ/` 与 `Additional file 1.docx`，
**不读** `data/processed/`。该陈旧目录已加弃用声明（`_DEPRECATED_勿用_修正前数据_20260917.md`），
保留仅作历史对照。

## 复现结果（2026-09-17 核验，与稿件逐值一致）

| 产物 | 复现值 | 状态 |
|---|---|---|
| Fig. 3 | 一致率 68.8%，ρ = 0.3896，max absZ = 2.83 | 一致 |
| Fig. 4 | Table 3b 三臂 ρ = 0.490 / 0.414 / 0.442；Δρ(dual−panel) = −0.020 [−0.127, +0.090]，P = 0.76；Δρ(dual−tissue) = +0.034 [−0.214, +0.280]，P = 0.78 | 一致 |
| Fig. 5 | TUBB eQTLGen +11.89（原图 +48.5）；CKAP4 +0.99（原图 +4.75） | 一致 |
| Fig. 6 | GTEx housekeeping 逐表型 4/29 + 0/29 + 1/29 = 5/87（= Table S7） | 一致 |
| Fig. 7 | RNH1 GTEx Nerve_Tibial +2.67 / +1.70 / +1.63 | 一致 |
| Fig. 8 | 0.414 (n = 138) / 0.636 (n = 72) / 0.418 (n = 8,890) | 一致 |
| SCZ 分母 | 基因池 10,357 / panel 9,048 / tissue 8,890 / 四臂 complete-case 8,315 | 一致 |

旧图备份于：

```
定稿图集_Fig1-8_20260914/_backup_before_officialZ_redraw_20260917/
```

## 未覆盖项

- **Fig. 2**（协变量平衡 SMD 图）：绘制的是基因长度 / GC 含量 / eQTL SNP 数，不含 Z 值，
  推测不受本次问题影响，但尚未逐值核验。
- 原 `figure_scripts/` 目录保留未删除。若后续有人误用其中脚本，会重现本次 P0 问题；
  建议在下次归档前删除或整体改名为 `_superseded_figure_scripts/`。

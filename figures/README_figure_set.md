# figures/ — the authoritative figure set

These files use the numbering of the manuscript (BMC Genomics submission):

- **Fig1–Fig8** — main-text figures
- **FigS1–FigS6** — Additional file 1 (supplementary) figures

Every figure is provided as **PNG (600 dpi, RGB)** and **PDF (vector)**, with an accompanying
`*_caption.txt` carrying the exact caption used in the manuscript — 43 files in total.

The files here are **byte-identical** to the finalized figure set of the submission
(`FigS1_caption.txt` … `FigS6_caption.txt` are identical to the captions embedded in
Additional file 1). Since 2026-09-20 the supplementary figures are **also** distributed from
this directory (previously only `FigS1` was), so that the archived repository fulfils the
manuscript's own statement: *"Supplementary figures are embedded in this file; high-resolution
PNG and PDF copies are deposited in the archived repository (Data availability)."*

## What changed, and why

| Date | Change |
|---|---|
| 2026-09-11 | An earlier generation carried `Figure1`–`Figure8` under a scheme in which the diagnostic-scheme figure sat at position 2, shifting every later number by one. Those files (and `Figure8.svg`) are superseded and removed — the PDFs here are vector. |
| 2026-09-20 | **Whole set re-synchronised** with the finalized submission files: Fig1–Fig8 (format batch: page width ≤170 mm, minimum font 6 pt, non-Type3 fonts, RGB PNG; Fig. 1 module 4 gains the "diagnostic partition" qualifier; Fig. 6 labels de-overlapped) and **FigS2–FigS6 added**; FigS1 caption aligned with Additional file 1. |

## How to regenerate

Use **`figure_scripts_officialZ_20260917/`** — the only pipeline that reproduces these files.
It reads `data/processed_officialZ/` (the official MetaXcan v0.8.1 Z recompute) and the tables of
Additional file 1, with all paths routed through `figure_scripts_officialZ_20260917/paths_config.py`:

```bash
cd figure_scripts_officialZ_20260917
python paths_config.py          # prints the resolved paths; override with env vars if needed
python 01_redraw_Fig5_Fig7.py   # …02, 04, 06, 08, 10 as listed in that directory's README
```

> ⚠️ **Do not** use `_DEPRECATED_figure_scripts_pre20260917/`: those five scripts read the
> pre-correction data layer `data/processed/` and reproduce retired values
> (RNH1 DR Z = 13.318 vs 2.3064; TUBB = 48.516 vs 11.8932; CKAP4 = 4.7549 vs 0.9874).
> Also **do not** use `data/processed/candidate_comparison_DR.csv` or `tables/Table1.csv`
> (2026-07-25 pipeline).

---

*Figure numbering, captions and the provenance of every panel are documented in the manuscript and in
`figure_scripts_officialZ_20260917/README.md`.*

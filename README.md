# TWAS eQTL Weight-Source Discordance — Two-Axis Dual-Source Audit

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.21238202.svg)](https://doi.org/10.5281/zenodo.21238202)

## Overview

This repository contains analysis scripts and processed data for:

**"Conventional TWAS endpoints cannot quantify the effect of eQTL weight-source choice on gene candidacy: a two-axis dual-source audit with disease-agnostic calibration and a genome-wide benchmark"**

Status: code & data release accompanying the manuscript (BMC Genomics submission).

> **Repository renamed on 2026-09-19** from `TWAS-eQTL-source-confounding` to `twas-eqtl-source-discordance`, to match the terminology adopted in the current manuscript. GitHub redirects the old name; the Zenodo archive, its concept DOI and all earlier release tags are unaffected. Update any existing clone with
> `git remote set-url origin git@github.com:wu-yijing/twas-eqtl-source-discordance.git`.

> The original v1.0.0 release (Zenodo [10.5281/zenodo.21428347](https://doi.org/10.5281/zenodo.21428347)) corresponded to the iScience submission titled *"eQTL source confounding systematically biases TWAS cross-population replication …"* (HOTAIR binding proteins, 104 genes, three diabetic complications). The archive now resolves through the concept DOI [10.5281/zenodo.21238202](https://doi.org/10.5281/zenodo.21238202), which always points to the latest version; the current version is v2.5.0 ([10.5281/zenodo.22752073](https://doi.org/10.5281/zenodo.22752073)), which adds the harmonized eQTLGen recompute (three-way allele harmonization), the Fig. S4 model-SNP-count panel on that recompute, and the resolved provenance of the dual-mismatch arm (GTEx multi-tissue Stouffer weighted-Z, not ACAT-O).

### Core Question

By how much does the choice of eQTL weight source (GTEx v8 tissue-specific vs. eQTLGen large-scale whole blood) perturb the conclusions a TWAS actually reports — Z-score sign, candidate gene rank and FDR gene candidacy — and how much of that perturbation is attributable to the resource/sample-size axis rather than the tissue-context axis?

> **Terminology:** throughout this repository and the current manuscript, *eQTL-source discordance* denotes the systematic perturbation of TWAS gene-candidacy conclusions induced by the weight source when GWAS input and analytic pipeline are held fixed. It is operational and **not** a claim of confounding in the causal sense — the earlier releases of this archive were described with the latter term, which the manuscript no longer uses.

### Figure set and figure scripts (current as of 2026-09-20)

**`figures/`** holds the complete, finalized figure set of the manuscript — **Fig1–Fig8** (main text) and
**FigS1–FigS6** (Additional file 1) — each as `PNG` (600 dpi, RGB) + `PDF` (vector) + a `*_caption.txt`.
All 43 files are **byte-identical** to the submission set; the in-figure numbering follows the manuscript
(no legacy `Figure1–Figure9` scheme). See `figures/README_figure_set.md`.

**`figure_scripts_officialZ_20260917/`** is the **only** pipeline that reproduces those figures: it reads
`data/processed_officialZ/` (official MetaXcan v0.8.1 Z recompute) and Additional file 1's tables.
All paths go through `paths_config.py` — run `python paths_config.py` to print the resolved
input/output paths, and override with `TWAS_REPO` / `TWAS_DATA_Z` / `FIG_OUT_MAIN` / `FIG_OUT_SUPP` /
`AF1_DOCX` / `FIG_RESULTS`. **Additional file 1 is not distributed with this repository** (it is the
journal's supplementary file); download it and point `AF1_DOCX` at it. See that directory's `README.md`.

Two directories are **superseded and kept only for audit** — do not use them to reproduce the paper:

| Directory | Why deprecated |
|---|---|
| `_DEPRECATED_figure_scripts_pre20260917/` | The five data-driven scripts of the 2026-09-14 generation. They read the **pre-correction** data layer `data/processed/` (missing the σᵢ expression-variance factor and with a PLINK 2-bit decoding defect), so they reproduce *retired* values — e.g. RNH1 DR Z = 13.318 vs 2.3064, TUBB = 48.516 vs 11.8932, CKAP4 = 4.7549 vs 0.9874 (×4–×6). |
| `_DEPRECATED_supplementary_old_numbering/` | The pre-2026-09-15 supplementary-figure scheme (`FigureS1–FigureS8`), which does **not** correspond to the current Additional file 1 (Fig. S1–S6). Superseded by `figures/FigS1–FigS6`. |

### Key Findings

> ⚠️ **Version note (2026-09-19, updated 2026-09-20).** The headline numbers in the table below predate the official-MetaXcan Z recompute archived here (`data/processed_officialZ/`). They describe the **v2.5.0 generation**. **The recomputed figure set has now been folded in (2026-09-20)**: `figures/` carries the current Fig1–Fig8 + FigS1–FigS6 produced by `figure_scripts_officialZ_20260917/` (see *Figure set and figure scripts* above). Treat `data/processed_officialZ/_PROVENANCE.json` and the files it records as authoritative for Z-scores, direction consistency and cross-cohort heterogeneity. On 2026-09-20 the two data-layer files `gtex_official_Z.csv` (FDR q columns, 217/222 rows) and `crosscohort_TableS4_official.csv` (pooled Z, Q, I², row labels) were also regenerated from Additional file 1 (Tables S2 and S4) after they were found to be one generation stale; figures are unaffected by that correction.

> **Note (2026-09-09, harmonized recompute):** The table below reflects the **current manuscript version (BMC Genomics submission)**, in which all eQTLGen-arm values were recomputed after **three-way allele harmonization** (eQTLGen assessed/other alleles ↔ FinnGen R13 ref/alt ↔ 1000G EUR A2) using the official eQTLGen FDR < 0.05 weight catalogue. This supersedes both the v1.0.0 (iScience) pipeline values and the earlier archived eQTLGen export, which contained an allele-alignment defect (24.1% of Z-scores flipped sign after harmonization). FDR enrichment rates are computed by Benjamini–Hochberg correction within each group × phenotype × source stratum; 37 of the 104 testbed genes have no eQTLGen model in the official FDR < 0.05 catalogue and are disclosed in the manuscript Methods; of the 67 that do, 61 return a valid S-PrediXcan statistic and form the eQTLGen arm (Additional file 1: Table S10). See `data/processed/eqtlgen_spredixcan_harmonized_results.csv` (gene-level Z-scores) and `data/processed/enrichment_comparison_harmonized.csv` (stratum-wise enrichment rates with Clopper–Pearson CIs).

| Metric | Value | Notes |
|--------|-------|-------|
| Total genes analyzed | 104 (30 candidate + 44 non-candidate + 30 T2DM control) | 104 records in covariate matrix (one row per gene) |
| Spearman ρ (GTEx WB vs harmonized eQTLGen Z-scores) | 0.32 (P = 0.001) | 96 gene–phenotype pairs from 32 genes (primary arm) |
| Direction consistency (primary arm) | 63.5% (61/96) | Exact binomial P = 0.010 vs 50%; DR 65.6%, DN 65.6%, DPN 59.4% |
| Direction consistency by gene-subset definition (manuscript Table 3c) | 62.7% (64/102) / 63.5% (61/96) / 69.9% (109/156) | both-source anchor set (34 genes) / primary arm (32 genes) / full testbed (52 genes); subset definitions shift estimates by ≤7 pp |
| GTEx FDR rate (candidate, DR) | 48.1% (13/27) | GTEx v8 MASHR, multi-tissue Stouffer integration |
| eQTLGen FDR rate (candidate, DR) | 41.7% (10/24) | Harmonized recompute; group rates 41.7–66.7% across phenotypes, comparable to non-candidate and T2DM controls |
| RNH1 GTEx Z-score (DR, Nerve_Tibial) | +13.82 | S-PrediXcan; from sparse 3-SNP MASHR model |
| RNH1 eQTLGen Z-score (DR) | +13.32 | Harmonized recompute; source-stable signal |
| Cross-cohort heterogeneity (RNH1, DR; FinnGen R13 vs UKB GCST90043640, both eQTLGen weights) | I² = 20.5% | Random-effects pooled Z = +1.52 (SE 0.79; P = 0.056), 95% prediction interval −0.33 to +3.36 (spans zero). Both cohorts direction-consistent (+2.31 and +0.72). Computed with the unmodified official MetaXcan v0.8.1 binary; √N_e-weighted re-merge gives Z = +2.39, Q = 0.13, I² = 0%. The former second cohort `ieu-b-4803` was withdrawn on 2026-09-16 (accession not resolvable in IEU OpenGWAS). |
| SCZ replication (n=2,511 pairs) | 68.3% direction consistency | Axis difference not significant (bootstrap P = 0.530) |

## SCZ 2×2 Decomposition Replication (added in v2.0.0)

A dual-source 2×2 decomposition was applied to schizophrenia (SCZ, PGC3 wave3 European) to test whether the source/panel vs tissue-context structure generalizes beyond the metabolic discovery cohort.

- **Method**: self-implemented TWAS Z = (w′R⁻¹z)/√(w′R⁻¹w) with LD from 1000G EUR, ridge RIDGE=0.1; 2×2 axes = source (eQTLGen Whole_Blood vs GTEx multi-tissue Stouffer) and tissue (GTEx Whole_Blood vs GTEx Nerve_Tibial).
- **Results (n = 2,511 gene–tissue pairs)**:
  - Source axis ρ = +0.522 (68.3% direction concordance)
  - Tissue axis ρ = +0.509 (71.2% direction concordance)
  - Both axes positive and comparable in magnitude — the axis ordering is **trait-dependent**, not a fixed "source always dominates" rule.
- **Sparsity robustness**: GTEx v8 MASHR models are sparse (median 2 SNPs/gene). Stratifying by model size (2 / 3–4 / ≥5 SNPs) gave stable ρ (≈0.5 across strata) with bootstrap 95% CI excluding zero; the eQTLGen side (median 786 SNPs/gene) anchors the source axis. See `scz_replication/results/`.
- **Scripts**: `scz_replication/scz_twas.py` (TWAS + decomposition), `scz_replication/build_weights_db.py` (weight DB), `scz_replication/robustness_scz.py` (stratified robustness), `scz_replication/make_figure9_scz.py` + `scz_replication/make_robustness_fig.py` (figures).
- **Figures**: the SCZ arm is not part of the current figure set (`figures/`). The two SCZ figure scripts (`make_figure9_scz.py`, `make_robustness_fig.py`) are iScience-era leftovers that write to `submission_iScience_v2/figures/` and `scz_replication/_rdat_tmp/` — neither path is tracked here. The machine-readable SCZ results are in `scz_replication/results/`.

## Quick Start (Docker — Recommended)

```bash
# 1. Build the Docker image
docker build -t twas-eqtl-repro .

# 2. Run the full reproducibility pipeline
docker run --rm -v $(pwd)/output:/app/output twas-eqtl-repro

# 3. View outputs
open output/figs/    # All figures in PDF + PNG
open output/logs/    # Analysis provenance logs
```

> ⚠️ **What the container actually runs.** `run_all.sh` executes the **early-generation** downstream scripts in `scripts/python/` (they write `figs/`, which is then copied to `output/`). The container therefore reproduces the environment and that earlier pipeline — **not** the current figure set. To regenerate `figures/` use `figure_scripts_officialZ_20260917/`; its step 00 needs Additional file 1 (the journal supplementary file, not distributed here), so it cannot run in the container without you supplying that file.

### What the Docker image contains
- **Python 3.13 + R 4.5.2** with all dependencies (pandas, scipy, statsmodels, MatchIt, etc.)
- **MetaXcan** (for S-PrediXcan, run separately with `run_spredixcan.sh`)
- **Downstream analysis scripts** that generate all paper figures
- **Processed data** needed to reproduce figures from intermediate results

## Local Setup (Conda)

```bash
# With your local conda installation:
conda env create -f environment.yml
conda activate twas-eqtl

# Run downstream analysis
python scripts/python/04_generate_all_figures.py
```

## Reproducing the Full Pipeline (Optional)

To reproduce from raw GWAS data (requires large input files):

```bash
# Mount external data and run S-PrediXcan
docker run --rm \
  -v /path/to/finngen_data:/input/finngen \
  -v /path/to/gtex_models:/input/gtex \
  -v /path/to/eqtlgen_weights:/input/eqtlgen \
  -v /path/to/1000g_ref:/input/1000g \
  -v $(pwd)/output:/app/output \
  twas-eqtl-repro \
  bash /app/run_spredixcan.sh
```

## Repository Structure

```
├── README.md                       # this file
├── LICENSE (MIT)  .gitignore  .dockerignore  .zenodo.json
├── Dockerfile  environment.yml  requirements.txt  renv.lock
├── run_all.sh                      # pipeline entry point (Docker ENTRYPOINT)
├── run_spredixcan.sh               # S-PrediXcan reference (needs external data)
├── run_three_figs.py
│
├── figures/                        # CURRENT figure set — 43 files
│                                   #   Fig1–Fig8 (main text) + FigS1–FigS6 (Additional file 1),
│                                   #   each as PNG (600 dpi, RGB) + vector PDF + *_caption.txt
├── figure_scripts_officialZ_20260917/   # CURRENT figure pipeline — reproduces figures/
│   ├── paths_config.py             #   single path entry point (env-overridable)
│   ├── 00…08, 10, 11 *.py          #   00 = data layer; 01/02/04/06/08/10 = figures; 05/07/11 = checks
│   ├── m15_positive_control.json   #   input behind Fig. S6
│   └── fig4_bootstrap_officialZ.json   # output of 06 (B = 5,000, seed 20260915)
│
├── data/
│   ├── processed_officialZ/        # AUTHORITATIVE Z / denominator layer (built from Additional file 1)
│   │   └── _PROVENANCE.json
│   ├── processed/                  # early-generation intermediates — DEPRECATED for Z-based results
│   │                               #   (file-level notices inside; see data/README.md)
│   ├── hk_reselect_20260830/       # control-layer (housekeeping) selection data
│   └── raw/                        # NOT tracked — raw public GWAS/eQTL data, too large for git
│
├── scripts/
│   ├── python/                     # EARLY-generation pipeline (provenance only)
│   │                               #   reads data/processed/, writes figs/; superseded by
│   │                               #   figure_scripts_officialZ_20260917/ — see its README_DEPRECATED.md
│   └── R/run_mahalanobis_matching.R
├── scz_replication/                # genome-wide PGC3 SCZ 2×2 arm — code + results/ (JSON + per-gene CSV)
├── analyses/                       # control-layer scripts + logs/ (run provenance)
├── tables/                         # exported table CSVs
├── analysis_reports/               # dated audit notes (accession checks, provenance)
│
├── _DEPRECATED_figure_scripts_pre20260917/   # the five 2026-09-14-generation figure scripts
└── _DEPRECATED_supplementary_old_numbering/  # pre-2026-09-15 supplementary scheme (FigureS1–FigureS8)
```

**Which part is current.** The paper's figures are `figures/`, and the only pipeline that reproduces
them is `figure_scripts_officialZ_20260917/` (start with `python paths_config.py`, which prints every
input and output path). The Z-scores, denominators and cross-cohort values are authoritative in
`data/processed_officialZ/`. Older generations are kept for provenance and marked
`_DEPRECATED_…`, `scripts/python/` (early-generation figure pipeline) or `data/processed/`
(pre-correction intermediates) — none of them should be used to reproduce the paper.
A directory named `figs/` is **not** part of the repository: `run_all.sh` creates it at run time as
its figure output, then copies it (with logs and processed data) into `output/`. `docs/`,
`manuscript/` and `figures/scz/`, which earlier versions of this README listed, do not exist.

## SCZ Replication Folder Layout

```
scz_replication/
├── scz_twas.py                  # TWAS Z + 2×2 decomposition (self-implemented)
├── build_weights_db.py          # assemble eqtlgen / GTEx WB / GTEx NT weight DB
├── robustness_scz.py           # sparsity-stratified robustness + bootstrap CI
├── make_figure9_scz.py          # Fig9 source/tissue scatter
├── make_robustness_fig.py       # FigS1 stratified bar chart
├── patch_manuscript_scz.py      # manuscript write-back (SCZ section)
├── patch_robustness.py          # manuscript write-back (limitation)
└── results/
    ├── scz_decomp_limit0.json  # decomposition ρ / n / concordance
    ├── scz_robustness.json     # stratified robustness + CI
    └── scz_twas_results_limit0.csv  # per-gene TWAS Z (10,357 genes)
```
The manuscript, cover letter and Additional file 1 are **not** part of this repository; the figure set that accompanies them is `figures/`. SCZ arm outputs: `scz_replication/results/`.

> Large intermediates (`weights.db`, extracted eQTLGen RDat weights, raw GWAS) are excluded by `.gitignore`; regenerate via the scripts above.

## Cluster-Robustness Re-analysis of Direction Consistency (S1)

The primary between-source comparison comprises 96 gene–phenotype pairs that derive from
only **32 genes**, so a gene's three phenotypes (DR, DN, DPN) are not independent
observations. The `scripts/python/s1_cluster_robustness/` folder quantifies this clustering
and recomputes the direction-consistency test with three cluster-aware procedures.

**Result: the intra-gene correlation is negligible (ICC = 0.000, design effect = 1.000).**
The exact binomial test reported in the manuscript (P = 0.010) is therefore **not materially
anti-conservative** — a cluster-robust sandwich test gives P = 0.013, a gene-level cluster
bootstrap gives P = 0.017 (95% CI 53.1–74.0%), and a gene-label permutation test gives
P = 0.008. All variants remain significant at α = 0.05, and the CI widens by only 0.9
percentage points (53.1–74.0% vs 53.1–73.1%).

For the genome-wide SCZ arm the complete-case set contributes **exactly one pair per gene**
(2,511 pairs from 2,511 genes), so the design effect is 1 by construction and no cluster
correction is required; the gene-resampling bootstrap coincides with the exact binomial test
(68.26%, ρ = 0.522 for the resource/sample-size axis; 71.21%, ρ = 0.509 for the
tissue-context axis).

Seeds: `20260910` (HOTAIR arms) and `20260726` (SCZ arm), B = 10,000 each.
Full methods, reproduction steps and a stale-file caveat are in
`scripts/python/s1_cluster_robustness/README.md`.

## Data Sources

All GWAS and eQTL summary statistics used in this study are from **publicly available sources**:

| Dataset | Source | Access |
|---------|--------|--------|
| FinnGen R13 (DR/DN/DPN) | [FinnGen](https://www.finngen.fi/) | Release 13 |
| GTEx v8 MASHR weights | [GTEx Portal](https://gtexportal.org/) | Public access |
| eQTLGen whole blood cis-eQTL | [eQTLGen](https://www.eqtlgen.org/) | Public access |
| 1000 Genomes EUR LD | [1000 Genomes](https://www.internationalgenome.org/) | Public access |
| UK Biobank DR (GCST90043640) | [IEU OpenGWAS](https://gwas.mrcieu.ac.uk/) | Public access |
| ~~UK Biobank DR (Xue et al. 2022; ieu-b-4803, 14,147 cases/322,390 controls)~~ | ⚠️ **Withdrawn 2026-09-16** — this accession is not present in IEU OpenGWAS (verified against the full 50,057-dataset catalogue; the neighbouring IDs `ieu-b-4795`–`ieu-b-4807` are likewise absent), and no local or repository copy exists; the Z value formerly attributed to it had no traceable data or code. The real DR subset of that publication (PMID 35841873) is GCST90134546 (1,652 cases / 60,577 controls) and has no public summary statistics. **Replaced by the already-used UKB DR dataset GCST90043640 (308 cases / 456,040 controls, re-analysed with eQTLGen weights).** See `analysis_reports/数据集编号溯源_Xue2022_ieu-b-4803.md`. |
| Cai et al. 2026 UKB T2D-DR | (in preparation) | Apply to UKB |
| Sakaue et al. 2021 DN (ebi-a-GCST90018832) | [IEU OpenGWAS](https://gwas.mrcieu.ac.uk/) | Public access |

## Requirements

### Python
```bash
pip install -r requirements.txt
```

Or with conda:
```bash
conda env create -f environment.yml
conda activate twas-eqtl
```

### R
- `matchit` (>= 4.5.0) — for Mahalanobis matching
- S-PrediXcan (v1.0, MetaXcan v0.8.1) — see [https://github.com/hakyimlab/MetaXcan](https://github.com/hakyimlab/MetaXcan)

## Usage

```bash
# Step 1: Run enrichment analysis and statistical tests
python scripts/python/03_enrichment_analysis.py

# Step 2: Generate figures
python scripts/python/01_visualize_mahalanobis_love_plot.py
python scripts/python/02_density_scatter_consistency.py
```

## Reproducibility

All processed data tables are provided in `data/processed/` and `scz_replication/results/`. Analysis scripts are version-controlled in this repository under MIT license. The repository snapshot is archived at Zenodo under the concept DOI [https://doi.org/10.5281/zenodo.21238202](https://doi.org/10.5281/zenodo.21238202), which always resolves to the latest version (current version v2.4.0, [10.5281/zenodo.22691102](https://doi.org/10.5281/zenodo.22691102)). The gene-level cluster-robustness re-analysis (S1) is archived both here (`scripts/python/s1_cluster_robustness/`) and in that Zenodo release as `s1_cluster_robustness.zip`. The sqrt(N_e)-weighted sensitivity re-merge of the RNH1 DR cross-cohort meta-analysis (M6(b)) and the descriptive three-study merge re-including GCST90043640 (M6(d)) are reproducible via `scripts/python/m6_ne_weighted_sensitivity.py` (output: `data/processed/m6_ne_weighted_sensitivity_results.txt`); the per-gene eQTLGen BH-FDR values underlying the DR rows of Table 1 are provided in `data/processed/eqtlgen_DR_pergene_FDR.csv`.

> ⚠️ `data/processed/eqtlgen_vs_gtex_comparison.csv` is **superseded for direction-consistency analyses**: it predates the three-way allele harmonisation, so its `Same_Direction` column reflects pre-harmonisation Z-scores. Use `data/processed/eqtlgen_spredixcan_harmonized_results.csv` instead.

## Citation

Wu Y, Chen M, Wu Q, Zhao J, Jin G. (2026). TWAS eQTL Source Confounding — Systematic Evaluation (v1.0.0). Zenodo. [https://doi.org/10.5281/zenodo.21428347](https://doi.org/10.5281/zenodo.21428347)

Wu Y. (2026). eQTL-source discordance in TWAS: a dual-source sensitivity analysis with true-negative calibration (code & data) (v2.4.0). Zenodo. [https://doi.org/10.5281/zenodo.22691102](https://doi.org/10.5281/zenodo.22691102)

Wu Y, Chen M, Wu Q, Zhao J, Jin G. (2026). TWAS eQTL weight-source discordance — two-axis dual-source audit (v2.6.0). Zenodo. Concept DOI [https://doi.org/10.5281/zenodo.21238202](https://doi.org/10.5281/zenodo.21238202) — *to be added once the corresponding Zenodo release is published (it always resolves to the latest version).*

## License

MIT License. See [LICENSE](LICENSE) for details.

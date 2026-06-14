# Composite Site Productivity Index (CSPI) analytical chain for the conterminous United States

Version 1.0.0 — 14 June 2026
Maintainer: Aaron R. Weiskittel, University of Maine, Center for Research on Sustainable Forests
ORCID: 0000-0003-2534-4478
License: CC-BY-4.0

## Companion deposit

This deposit is the analytical chain (manuscript drafts, intermediate result tables, pipeline scripts, outreach materials, and documentation) that supports the **CSPI data product** at Zenodo concept DOI **10.5281/zenodo.20515034**. The two deposits are linked: cite this one when reproducing methods or extending the multidim analyses; cite the data deposit when using the surfaces in modeling work.

## What this release contains

Version 1.0.0 of the CSPI analytical chain covers the v0.10g manuscript ("Beyond site index: FIA's own classification system disagrees with site index but agrees with a multi-dimensional composite of forest productivity at 30 m across the conterminous United States", in revision at Forest Ecology and Management). It includes:

### Manuscript

- `CSPI_v0_10_manuscript_draft.md` the v0.10g markdown source with bootstrap CIs, the FIA-on-FIA representation reframe, and the multilevel ecoregion analysis
- `FEM_combined.md` the cover letter + title page + highlights + manuscript + figures + supplements combined for FEM submission
- `CSPI_v10_FEM_combined.docx` the rendered 4 MB Word version with 11 embedded figures
- `manuscript_v0_10b_self_review.md` the manuscript-review skill self-audit (Modules 1, 2, 5)

### Analyses

Result CSVs from every analytical pass in the v0.10 development cycle:

- `analyses/multidim_v3/C1` through `C5` — disturbance, treatment, stand size, per-state correlations, SITECLCD prediction
- `analyses/multidim_v4/D1` through `D4` — SICOND base-age stratification and projection to base 50
- `analyses/multidim_v4/E1b`, `E2c` — NRCS SSURGO `coforprod` site index extraction
- `analyses/multidim_v4/G0` through `G4` — Cieszewski-Bailey GADA refit on the unified compilation
- `analyses/multidim_v4/H1` — bootstrap 95% CIs on the headline correlations
- `analyses/multidim_v5_L3/M1L3` through `M3L3` — Level III ecoregion PCA weights and SITECLCD prediction
- `analyses/multidim_v5_ML/MLM` — hierarchical L1/L2/L3 ecoregion weighting
- `analyses/multidim_v5_ML2/STRESS1` and `STRESS4` — bootstrap CIs and tau hyperparameter sensitivity for the multilevel ensemble

### Scripts

The R, Python, and SLURM scripts that produced every result, organized by analysis pass:

- Component model fits (ESI v5/v6/v7/v8, BGI, Asym)
- Multidim_v2 (4-measure complete-case) and v3 (FIA COND join)
- Multidim_v4: sicond_base50.r, ssurgo_login.r, ssurgo_join_fix.r, compilation_si.r, gada_refit.r, bootstrap_cis.r
- Multidim_v5: ecoregion_composite.r (state-grouped first cut), ecoregion_L3_composite.r, multilevel_ecoregion_full.r, multilevel_fast.r, multilevel_from_L3.r, bootstrap_multilevel.r
- Surface generation: cspi3c_postsurface.slurm, cspi3c_forest_mask.slurm, fig_F7_3c.r
- Figure builds: cspi_v10_figures.r, build_cspi_v10_collab_deck.py

### Outreach

External communication materials staged for coordinated release after manuscript acceptance:

- `outreach/01_crsf_news_article.md` long-form web feature for crsf.umaine.edu
- `outreach/02_social_posts.md` LinkedIn (3 variants) and X (2 variants) with cadence
- `outreach/03_press_release_draft.md` press release for UMaine Communications review
- `outreach/04_coauthor_review_email.md` review request for Anthony D'Amato and Jereme Frank
- `outreach/README.md` deployment cadence and tagging guidance

### Documentation

- `docs/SYNTHESIS_unified_cspi_recommendations.md` stakeholder synthesis with operational defaults by application
- `docs/PROTOTYPE_v3_ecoregion_results.md` v3.0.0 ecoregion prototype results memo
- `docs/STRESS_TEST_v0_10g.md` six weaknesses + mitigations for the multilevel finding
- `docs/REORG_cspi_repo_and_deposit.md` the plan for splitting CSPI from fvs-conus
- `docs/PAPER_SPLIT_outline.md` two-paper split recommendation (Paper 1 to FEM, Paper 2 to Forest Science or MEE)
- `docs/handoffs/HANDOFF_20260613_extended_autopilot.md`, `HANDOFF_20260614_v0_10d_full_session.md` session continuity notes

## Versioning

- v0.10b SICOND base-age Simpson paradox correction
- v0.10c NRCS SSURGO comparator addition
- v0.10d GADA refit confirmation
- v0.10e/f/g manuscript polish, ecoregion analysis, multilevel hierarchical extension
- v0.10g+ FIA-on-FIA representation reframe + bootstrap CIs + tau insensitivity (this deposit)

NEWS.md documents the full revision trace with transparency notes on the Simpson paradox correction.

## Source repository

Active development happens at [holoros/cspi-conus](https://github.com/holoros/cspi-conus). This Zenodo deposit is a frozen snapshot at the v0.10g+ point; future versions track the manuscript revision cycle.

## How to cite

For the analytical chain (this deposit):

> Weiskittel, A.R. (2026). Composite Site Productivity Index (CSPI) analytical chain v1.0.0 for the conterminous United States. Zenodo. https://doi.org/10.5281/zenodo.XXXXXXX

For the underlying data product (companion concept):

> Weiskittel, A.R. (2026). Composite Site Productivity Index (CSPI) v2.0.0 for the conterminous United States. Zenodo. https://doi.org/10.5281/zenodo.20663652

For the conceptual paper (in revision):

> Weiskittel, A.R. (in revision). Beyond site index: FIA's own classification system disagrees with site index but agrees with a multi-dimensional composite of forest productivity at 30 m across the conterminous United States. Forest Ecology and Management.

## Funding

USDA National Institute of Food and Agriculture, McIntire-Stennis Forestry Research Program (Maine Agricultural and Forest Experiment Station). University of Maine. Ohio Supercomputer Center allocation PUOM0008.

## Contact

aaron.weiskittel@maine.edu

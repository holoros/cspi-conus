# What's left for the FEM manuscript

Status snapshot, v0.10j manuscript, 15 June 2026.

## What just landed (this session)

ESI v9 (climate + parent material) at FIA plots, with and without LAT/LON in the predictor stack. Headline numbers: LAT/LON removal costs only 0.004 R² (climate covariates almost fully recover geographic information); pm_factor ranks last by impurity importance whether coordinates are included or not. Plot-resolution spatial diagnostic at 41,065 plots shows mean |ΔESI| = 0.22 m on a 28 m mean SI scale; only 1.1 percent of plots shift more than 1 m; only Organic peatlands (n = 413) show a meaningful structural reorganization (mean −0.25 m). The 30 m CONUS parent material raster (1.4 billion pixels, 255 MB compressed) is staged on Cardinal for v3.0.0 release as a separately citable overlay layer.

PM-aware composite weighting test: M1 equal-weight composite OOB R² = 0.800; M2 free 3-component = 0.817 (the +0.016 R² gain documented in §3.9); M3 free 3-component + pm_factor (additive) = 0.817 (PM adds essentially zero on top of free weights); M4 per-PM stratified n-weighted average = 0.774 (worse than pooled because small categories overfit). The interesting structural finding is per-PM weight heterogeneity: Alluvial and Residuum are BGI-dominated (impurity BGI ≫ ESI), Marine is ESI ≈ Asym > BGI, Colluvium and Glacial are ESI-dominated. The single global weighting hides this pattern but does not need to be replaced for operational use.

Manuscript bumped to v0.10j with §3.14 spatial diagnostic, Tables 7b and 7c, Figure 12 + 12b, and a methodological note on LAT/LON exclusion. Committed to holoros/cspi-conus (commit 9333cf4).

## Open items, ordered by what blocks submission

### Currently running

BGI and Asym v9 test, refit on the v7 training stack with bgi_v and asym_v joined from the multidim_v2 plot table. Same RF setup as ESI v9. SLURM job 11593395 in queue. When it lands the §3.14 paragraph will be extended with one or two sentences on whether BGI and Asym track the ESI null result (likely, given climate dominates productivity prediction in general) or whether the Organic peatlands kink shows up larger in BGI than in ESI (the hypothesis worth testing). Expected to land within the hour.

### Blocking pre-submission work

Three placeholder references still need verification: Westfall NSVB 2023, the regional spatial-structure 2024 reference cited in §4.7.1, and Smith carbon offset 2024 cited in §4.6.1. These were flagged [REVIEWER VERIFY] earlier and have not been resolved. Either confirm the DOIs and full citations or remove the citations from the text.

NIFA McIntire-Stennis project number placeholder in §6 Acknowledgments still reads [PLACEHOLDER: confirm specific NIFA McIntire-Stennis project number]. Aaron has this number; needs a 30 second lookup.

Coauthor review request. Email drafted to Anthony D'Amato and Jereme Frank in an earlier session. Awaits Aaron's send.

Pre-submission checklist refresh. The previous checklist was written at v0.10g; with v0.10i and v0.10j changes (geology promoted as headline, stand age moved to supplements, spatial diagnostic added) the checklist needs a one-page update before submission.

### v3.0.0 release work (after FEM acceptance, or in parallel)

Parent material 30 m CONUS raster: deposit to Zenodo as a separately citable overlay. Staged on Cardinal at /fs/scratch/PUOM0008/crsfaaron/cspi_v7/multidim_v6_geol/parent_material_30m_CONUS.tif.

ESI v7 no-coordinates surface prediction at 30 m CONUS. Current ESI v7 surface used the soil + terrain + canopy + microclimate + v6_distill predictor stack; the v9 refit reported here uses the climate-only ALL_SI_m stack. For the v3.0.0 surface release we need to decide whether the operational ESI v7 surface stays with the existing soil/terrain/canopy stack (which is already published) or moves to a climate-only ClimateNA 1 km stack (which would need a new surface job). The plot-level R² is comparable; the question is whether downstream users prefer the higher-resolution soil/terrain stack or the climate-only conceptual framing.

v2.0.1 follow-up deposit on Zenodo. Waits on v7_qrf array completion (currently held by AssocGrpJobsLimit; eight tasks still pending). Once it completes the v7_unc uncertainty layer ships with v2.0.1.

### Two-paper split decision

The outline for splitting Paper 1 (FEM, multi-dimensional productivity + SICOND + GADA) from Paper 2 (Forest Science or Methods in Ecology and Evolution, multilevel ecoregion composite) was drafted earlier. The current v0.10j manuscript still bundles both papers together. Aaron's executive call on whether to split before FEM submission or keep them bundled. If split, Paper 2 needs its own abstract and methods write-up.

### Optional strengthening analyses

Disturbance code (DSTRBCD1) stratification: does the multi-dim pattern hold across disturbed vs undisturbed plots? Would be a one-paragraph §3.15.

Treatment code (TRTCD1) stratification: same question across managed vs unmanaged. Likely combines with DSTRBCD1 in a single §3.15 paragraph.

Forest type group (FORTYPCD) stratification: more granular than species. Worth a Table S13.

Stand density (relative density, SDI, BAL) interaction: does the ESI-BGI orthogonality depend on stand density? Would test whether the multi-dim story scales across stocking levels.

Climate change vulnerability: do the four productivity measures disagree on climate sensitivity? Could be a Table S14 if Aaron's existing PERSEUS runs can be cross-referenced cheaply.

None of these are blocking. They are nice-to-have material if a reviewer asks "have you considered X" and Aaron wants to have an answer in hand.

## Recommendation

Wait for BGI/Asym v9 test to land in the next hour, integrate one paragraph into §3.14, then this manuscript is ready for the coauthor review pass. The blocking items above (three citations, one acknowledgment lookup, coauthor email send) are all sub-hour tasks. Optional strengthening analyses are deferred to v0.11 or to revision response after the FEM editor decision.

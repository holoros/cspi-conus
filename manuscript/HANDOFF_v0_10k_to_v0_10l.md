# CSPI Manuscript & v3.0.0 Release Handoff
## End of session 18 June 2026 → next session

This doc captures the full state for picking up where we left off. The prompt for the next session is at the bottom.

## Where we are

The CSPI multi-dimensional productivity manuscript is at draft **v0.10k**. It has been restructured to lead with the two-story arc Aaron requested: a primary story about cross-metric relationships and connection to traditional FIA SICOND/SITECLCD, and a secondary story about structural drivers of metric divergence (regional variation, species heterogeneity, biological implications). The new §3.18 (Disturbance × EPA L3 ecoregion interaction) is the strongest single piece of evidence in the paper that metric divergence is structural rather than methodological: joint stratification produces r(ESI,BGI) range of 1.40 across 115 cells, wider than any single-axis stratification.

The v3.0.0 surface release is most of the way there. The ASYM_V9_CONUS_30m.tif headline mosaic (90,000 × 212,400 pixels, 13 GB) is built and saved on Cardinal at `/users/PUOM0008/crsfaaron/raster_layers/asym_v9/`. The forest-mask + delta layer pipeline is currently running in a GDAL-streaming job after an R/terra OOM at 128 GB.

## What's in flight on Cardinal

Job **11741898** (`asym_gdal`). Streaming GDAL pipeline that produces three derived layers from the ASYM_V9_CONUS_30m.tif mosaic:

1. `asym_v7_resampled_30m.tif` (37.6 GB, already done) — v7 Asym 1km bilinearly warped to the v9 30m grid.
2. `ASYM_DELTA_v9_v7_CONUS_30m.tif` (in progress) — Float32 pixelwise v9 minus v7.
3. `ASYM_V9_CONUS_30m_forestmasked.tif` — v9 masked to the v7 forest CSPI footprint.
4. `ASYM_DELTA_v9_v7_CONUS_30m_forestmasked.tif` — delta layer masked to forest.

Expected completion: within the next 1 to 2 hours from job start. Resource: 64 GB / 8 cpu / 6 hr wall. Once these land the v3.0.0 release package is ready to fire.

## v3.0.0 release direction (clear, no decision needed)

Ship three new products alongside the v2.0.0 baseline:

1. `ASYM_V9_CONUS_30m_forestmasked.tif` — the headline corrective surface for asymptotic biomass. OOB R² = 0.836 vs v7 baseline 0.828, ΔR² = +0.0078, RMSE 8.40 → 8.21 Mg/ha. PM is the substantive driver.
2. `ASYM_DELTA_v9_v7_CONUS_30m_forestmasked.tif` — pixelwise difference layer for users who want to see where geology reorganizes Asym predictions.
3. `parent_material_30m_CONUS_4326.tif` (already built, 307 MB) — gSSURGO-derived 10-class parent material raster as a separately citable overlay.

Metadata template at `cspi-conus/zenodo/v3.0.0_metadata.json` is ready to fire via the `zenodo-deposit` skill.

## Tasks remaining for FEM submission

Listed in priority order. Sub-5-minute items in italics.

1. **Coauthor review email send** *(your call, drafted in earlier session)*. Sending to Anthony D'Amato and Jereme Frank with the v0.10k docx attached gets the manuscript moving toward submission.
2. **v3.0.0 deposit fire** (next session, autopilot-ready). Once the forest-mask + delta layers land, the deposit script runs and the v3.0.0 DOI is minted within an hour.
3. **Backfill v3.0.0 DOI into manuscript abstract + §7 + NEWS** *(autopilot-ready after Zenodo mints)*.
4. **NEWS.md v0.10k update** for the cspi-conus repo, summarizing the §3.18 result + two-story reframe + Asym v9 surface deposit.
5. **Optional final polish pass** — fact-check Tables 1, 2, 3, 7, 8, 9, 9c numbers against the underlying CSVs in `analyses/multidim_v6_geol/`. Already done for v0.10g; only Table 9c and §3.18 prose are new and would benefit from one more pass.
6. **Submit to Forest Ecology and Management** *(your action; the package is ready)*.

## What's done in this session

The autopilot push covered nine main items:

1. v9 BGI and Asym tests on parent material (PM is most useful for Asym, ΔR² = +0.006 on v7 stack, ΔR² = +0.008 on v3 stack).
2. PM-aware composite weighting (per-PM optimal weights heterogeneous: Alluvial+Residuum BGI-dominated, Marine ESI≈Asym, Colluvium+Glacial ESI-dominated).
3. Five strengthening stratifications (DSTRBCD1, TRTCD1, FORTYPCD, STDSZCD, plus the headline DSTRB × L3 interaction).
4. Three blocking citation placeholders verified and fixed (Westfall NSVB GTR WO-104 2024, Subedi 2024 FEM, Hoover and Smith 2012 not Smith 2024).
5. NIFA McIntire-Stennis acknowledgment removed (Aaron has no MAFES appointment).
6. ASYM_V9_CONUS_30m.tif tile-array predict + mosaic (240 tiles, 30m CONUS).
7. v0.10k manuscript reorganization to two-story arc (Abstract, §3 dividers, §3.18 new, Conclusion).
8. docx + PPTX regenerated for v0.10k (CSPI_v0_10k_FEM_combined.docx, CSPI_v10k_collaborators_overview.pptx).
9. Forest-mask + delta layers in-flight via GDAL streaming.

Commits on holoros/cspi-conus: 9333cf4, 381fb17, 4acb1f4, 5743b2d, 8c39a4e, 58ed07d, 1afacba.

## Key paths on Cardinal

| What | Where |
|---|---|
| Asym v9 model | `/fs/scratch/PUOM0008/crsfaaron/cspi_v7/multidim_v6_geol/m_asym_v9_v3stack.rds` |
| Asym v9 30m mosaic | `/users/PUOM0008/crsfaaron/raster_layers/asym_v9/ASYM_V9_CONUS_30m.tif` |
| Asym v9 forest-masked | (in flight) `ASYM_V9_CONUS_30m_forestmasked.tif` same dir |
| Asym delta layer | (in flight) `ASYM_DELTA_v9_v7_CONUS_30m.tif` same dir |
| Parent material 30m EPSG:4326 | `/fs/scratch/PUOM0008/crsfaaron/cspi_v7/multidim_v6_geol/parent_material_30m_CONUS_4326.tif` |
| GADA SI plot extracts | `/fs/scratch/PUOM0008/crsfaaron/cspi_v7/multidim_v5_L3/` |
| Plot-level diagnostics | `/fs/scratch/PUOM0008/crsfaaron/cspi_v7/multidim_v6_geol/PM*.csv`, `S*.csv` |
| Predict tile scripts | `/users/PUOM0008/crsfaaron/fvs-conus/R/eval/cspi_v3/predict_asym_v9_tiled.r` |
| Post-surface streaming GDAL | `/users/PUOM0008/crsfaaron/fvs-conus/R/eval/cspi_v3/asym_v9_postfix_gdal.sh` |

## Key paths locally

| What | Where |
|---|---|
| Manuscript | `/home/aweiskittel/Documents/Claude/CRSF-Cowork/active-projects/bgi-cspi-conus/v5/CSPI_v0_10_manuscript_draft.md` (now at v0.10k content) |
| docx | `CSPI_v0_10k_FEM_combined.docx` |
| PPTX | `CSPI_v10k_collaborators_overview.pptx` |
| Zenodo v3 metadata | `cspi-conus/zenodo/v3.0.0_metadata.json` |
| NEWS v3.0.0 | `cspi-conus/NEWS_v3.0.0.md` |
| v3 release path forward | `v3_release_path_forward.md` (now mostly superseded by this handoff) |

---

## PROMPT FOR NEXT SESSION

Paste this verbatim into the next session to pick up where we left off.

> The CSPI multi-dimensional productivity manuscript is at draft v0.10k with the two-story arc reorganization Aaron requested (primary story: cross-metric vs traditional FIA SICOND/SITECLCD + CSPI v2 development; secondary story: drivers of metric divergence including the new §3.18 disturbance × ecoregion interaction). The v3.0.0 surface release is most of the way to deposit-ready: ASYM_V9_CONUS_30m.tif is built (90k × 212k pixels, 13 GB), and the forest-mask + delta layer pipeline is in flight on Cardinal as job 11741898 via a streaming GDAL pipeline. Full state is documented at `/home/aweiskittel/Documents/Claude/CRSF-Cowork/active-projects/bgi-cspi-conus/v5/HANDOFF_v0_10k_to_v0_10l.md`. Please pick up by:
>
> 1. SSH into Cardinal and verify job 11741898 (`asym_gdal`) completed cleanly. Check sacct, the log tail, and that `ASYM_V9_CONUS_30m_forestmasked.tif`, `ASYM_DELTA_v9_v7_CONUS_30m.tif`, and `ASYM_DELTA_v9_v7_CONUS_30m_forestmasked.tif` all exist in `/users/PUOM0008/crsfaaron/raster_layers/asym_v9/`.
> 2. If the streaming GDAL job hit disk space (the intermediate `asym_v7_resampled_30m.tif` is 37.6 GB), free space on scratch and resubmit just the delta step.
> 3. Once all four output rasters are present, fire the v3.0.0 Zenodo deposit using the `zenodo-deposit` skill and the metadata at `cspi-conus/zenodo/v3.0.0_metadata.json`. Upload: ASYM_V9_CONUS_30m_forestmasked.tif, ASYM_DELTA_v9_v7_CONUS_30m_forestmasked.tif, parent_material_30m_CONUS_4326.tif, m_asym_v9_v3stack.rds, NEWS_v3.0.0.md.
> 4. Once Zenodo mints the v3.0.0 DOI, backfill it into the manuscript Abstract, §7 Data and Code Availability, NEWS, and Zenodo concept record.
> 5. Update the cspi-conus repo NEWS.md with the v0.10k two-story reorganization, the §3.18 result, and the v3.0.0 surface release. Commit and push.
> 6. Refresh the v0.10k docx and PPTX with the minted v3.0.0 DOI.
> 7. Confirm everything is ready for Aaron to send to coauthors (Anthony D'Amato, Jereme Frank). The coauthor review email was drafted in an earlier session; locate it in the project folder and verify it references the right docx version.
>
> If at any step Cardinal job state is ambiguous, write a short status note and ask Aaron how to proceed before doing destructive operations. Per Aaron's security policy: file deletions on Cardinal are prohibited (provide rm commands as text only); Zenodo token at `~/.zenodo_token` on Cardinal mode 600 — never echo to stdout; GitHub PAT at `/sessions/.../.gh-holoros/token`; never place tokens in URL query params (use Authorization Bearer header); IAM permission grants are user-only.
>
> The work is funded by the University of Maine Center for Research on Sustainable Forests (CRSF) only. Aaron does not have a MAFES appointment so do not reattribute to NIFA McIntire-Stennis under any circumstances.
>
> Style preferences: retain past knowledge and provide R code when possible, Python when useful. Do not use hyphens (they are a clear AI tell in prose).

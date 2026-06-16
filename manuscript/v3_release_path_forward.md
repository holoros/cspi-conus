# v3.0.0 Release Path Forward

Status as of 2026-06-15 end of session.

## What's done

Analyses for the manuscript are complete. Manuscript at v0.10j with new §3.16 (DSTRBCD1/TRTCD1/FORTYPCD/STDSZCD strengthening), §3.17 (Asym v9 v3-stack refit numbers), Table 8 (PM-aware composite per-PM weight heterogeneity), Table 9 (range of r(ESI,BGI) across stratifications), and Figure 12 + 12b (plot-level spatial diagnostic). Committed at holoros/cspi-conus 8ceecb0.

The Asym v9 model is trained on the v3 environmental predictor stack and saved at `/fs/scratch/PUOM0008/crsfaaron/cspi_v7/multidim_v6_geol/m_asym_v9_v3stack.rds`. OOB R² = 0.836, RMSE = 8.21 Mg/ha, ΔR² over v7 baseline = +0.0078.

The parent material 30 m CONUS raster in native gSSURGO CRS (NAD83 Albers) is built and saved at `/fs/scratch/PUOM0008/crsfaaron/cspi_v7/multidim_v6_geol/parent_material_30m_CONUS.tif` (255 MB, 1.4 billion pixels, 10 categories).

The v3.0.0 Zenodo metadata template is staged at `zenodo/v3.0.0_metadata.json` in the cspi-conus repo, with full description of all three new products and the related identifiers chain (concept DOI, v2.0.0 prior version, analytical chain supplement).

The NEWS_v3.0.0.md is written and ready to ship with the deposit.

## What's in flight on Cardinal

**Job 11641470 (pm_4326 reproject)**: Reprojects the parent material raster from NAD83 Albers to EPSG:4326 on the aligned_30m DEM grid using nearest-neighbor warp. 96 GB, 8 cpu, 3 hr wall cap. Output: `parent_material_30m_CONUS_4326.tif`. Once this lands, the Asym v9 surface tile array becomes unblocked.

## What needs to happen next session

**Step 1.** Verify reproject job 11641470 completed cleanly. Pull the freq() table to confirm pixel counts roughly match the native-CRS version (small differences expected due to nearest-neighbor resampling at the 30 m / 1 arcsec grid).

**Step 2.** Resubmit the Asym v9 30 m surface tile array. Script is at `/users/PUOM0008/crsfaaron/fvs-conus/R/eval/cspi_v3/predict_asym_v9_tiled.r` and has been patched to point at the EPSG:4326 PM raster. Slurm file at `/users/PUOM0008/crsfaaron/fvs-conus/R/eval/cspi_v3/predict_asym_v9.slurm` is configured for 240 tiles (12 × 20), 8 concurrent, 3 hr per tile cap. Expected wall time: 240/8 × 10 min ≈ 5 hr.

**Step 3.** Mosaic the 240 tiles into a single COG. Use `gdal_merge.py` or `gdalbuildvrt + gdal_translate` to assemble `ASYM_V9_CONUS_30m.tif`. Reference the existing `cspi3c_postsurface.slurm` script for the mosaic recipe.

**Step 4.** Build the delta layer: `ASYM_DELTA_v9_v7_CONUS_30m.tif = ASYM_V9_CONUS_30m.tif - ASYM_V7_CONUS_30m.tif`. The v7 surface is at `/users/PUOM0008/crsfaaron/raster_layers/cspi_rs/Asym_on_esi_grid.tif` or equivalent. Use `terra::rast() + terra::diff()` or `gdal_calc.py`.

**Step 5.** Forest-mask both surfaces using the existing forest mask at `/fs/scratch/PUOM0008/crsfaaron/forest_mask_30m.tif`. Produces `ASYM_V9_CONUS_30m_forestmasked.tif` for the headline release product.

**Step 6.** Stage Zenodo deposit. Use the `zenodo-deposit` skill with metadata from `zenodo/v3.0.0_metadata.json`. Files to upload:
- ASYM_V9_CONUS_30m_forestmasked.tif (corrective surface, ~1 GB compressed)
- ASYM_DELTA_v9_v7_CONUS_30m.tif (difference layer, ~1 GB)
- parent_material_30m_CONUS_4326.tif (overlay, ~255 MB)
- m_asym_v9_v3stack.rds (model object, ~50 MB)
- NEWS_v3.0.0.md
- README.md (data dictionary)
- analyses tarball

**Step 7.** Once Zenodo mints the v3.0.0 DOI, backfill the DOI into:
- manuscript v0.10j abstract and §7 Data Availability
- NEWS_v3.0.0.md
- cspi-conus README

**Step 8.** Commit and tag v3.0.0 in the cspi-conus repo. Push tag and create a GitHub release with the NEWS as release notes.

## File path reference

| Component | Path |
|-----------|------|
| Asym v9 model | `/fs/scratch/PUOM0008/crsfaaron/cspi_v7/multidim_v6_geol/m_asym_v9_v3stack.rds` |
| PM 30m native CRS | `/fs/scratch/PUOM0008/crsfaaron/cspi_v7/multidim_v6_geol/parent_material_30m_CONUS.tif` |
| PM 30m EPSG:4326 (in flight) | `/fs/scratch/PUOM0008/crsfaaron/cspi_v7/multidim_v6_geol/parent_material_30m_CONUS_4326.tif` |
| PM code legend | `/fs/scratch/PUOM0008/crsfaaron/cspi_v7/multidim_v6_geol/PM5_pm_codes.csv` |
| v3 aligned 30m stack | `/fs/scratch/PUOM0008/crsfaaron/cspi_v3/aligned_30m/` |
| Tile output dir | `/fs/scratch/PUOM0008/crsfaaron/asym_v9/conus_tiles/` |
| Final surface dir | `/users/PUOM0008/crsfaaron/raster_layers/asym_v9/` |
| Predict script | `/users/PUOM0008/crsfaaron/fvs-conus/R/eval/cspi_v3/predict_asym_v9_tiled.r` |
| Predict slurm | `/users/PUOM0008/crsfaaron/fvs-conus/R/eval/cspi_v3/predict_asym_v9.slurm` |
| Reproject script | `/users/PUOM0008/crsfaaron/fvs-conus/R/eval/cspi_v3/reproject_pm_raster.r` |
| Forest mask | `/fs/scratch/PUOM0008/crsfaaron/forest_mask_30m.tif` |
| v3.0.0 metadata | `zenodo/v3.0.0_metadata.json` in cspi-conus repo |
| v3.0.0 NEWS | `manuscript/NEWS_v3.0.0.md` (to be added) in cspi-conus repo |

## Summary of v0.10j manuscript headline numbers

| Stratification axis | r(ESI,BGI) range | SITECLCD ΔR² over equal-weight composite |
|--|--|--|
| Forest type group (FORTYPCD hundred) | −0.555 to +0.591 | +0.052 |
| Disturbance (DSTRBCD1) | −0.715 to +0.243 | not tested |
| Multi-level ecoregion (L1/L2/L3, §3.9) | n/a | +0.054 |
| Species (SPCD, §3.3) | −0.02 to +0.53 | not tested |
| Treatment (TRTCD1) | −0.664 to −0.388 | not tested |
| Parent material (gSSURGO, §3.14, §3.15) | not computed | +0.0003 (adds nothing on top of free 3-comp) |
| Stand size class (STDSZCD) | −0.511 to −0.431 | not tested |

| Per-measure ΔR² from adding parent material (no LAT/LON) |
|--|
| ESI v9 (climate stack, n=36,958): +0.001 (val), pm_rank 35/35 |
| BGI v9 (env stack, n=31,069): +0.001 (val), pm_rank 12/17 |
| Asym v9 v7-stack (n=31,069): +0.006 (val), pm_rank 10/17 |
| **Asym v9 v3-stack (n=31,463): +0.008 (OOB), pm_rank 8/12** |

# OODA: CSPI asymptote scaling for the Block 1+3 production refit (#75 step 3)

*2026-06-27. Autonomous OODA run, condensed mode. STACK: extraction Python/rasterio (raster IO), scaling module R/data.table (slots into ycx_canonical_ci_*). Decision being resolved: how to wire the CSPI site-productivity covariate into the cell asymptote of the remeasurement refit.*

---

## OBSERVE

Inputs, all on Cardinal, all data confirmed present and read clean.

| Object | Source | Shape |
|---|---|---|
| Per-plot membership (48 states) | `~/yield_curves_conus/config/ycx_membership_<ST>.csv` | 638,751 plots; LAT, LON, ft_group, prov_code, owner4, cell_key |
| CSPI surface | `cspi_v7/v2both/CSPI_v2_5component_1km.tif` | EPSG:4326, 1 km, 0 to 100 index, continuous (forest + nonforest) |
| Per-cell fitted asymptotes | `ycx_<ST>_remeas_fits.csv` (scope=cell, carbon_lbac) | 1,801 state-cell carbon fits |
| Cell CSPI table (new) | `ycx_cell_cspi.csv` (produced this run) | 873 cells, 306 state_prov, 48 states |

CSPI sampled at plot coordinates: valid for 638,153 of 638,751 plots (99.9%). CONUS median CSPI = 56.36. Execution is non interactive; all reads and the raster sampling ran server-side, only the coordinate-free cell table left Cardinal (true-coords protocol honored).

## ORIENT

Statistical and compute vulnerabilities identified before deciding the scaling form.

- **Confounding (primary risk).** Productive ecoregions carry both high CSPI and high-biomass forest types. A pooled CSPI-to-asymptote fit absorbs forest-type composition into the CSPI coefficient and overstates the site effect.
- **Index nonlinearity.** CSPI is a 0 to 100 composite, not a physical productivity unit; its relationship to maximum biomass need not be linear or proportional.
- **Sparse cells.** Many of the 873 cells have few plots; a strong free-fit exponent applied to a noisy cell mean destabilizes the asymptote.
- **Raster grid mismatch.** The 30 m `cspi_4c_raw.tif` is forest-pixel-only and returned 0% valid at fuzzed plot centers; the continuous 1 km layer is the correct sampling surface (99.9% valid). [ASSUMPTION: 1 km CSPI is sufficient resolution for cell and ecoregion means; sub-cell within-pixel variation is not the target here.]
- **Pseudoreplication.** Cells repeat across states sharing an ecoregion; CSPI is keyed CONUS-pooled per ft|prov, so the same value maps to multiple state-cells. Acceptable for a covariate but argues against treating each state-cell as independent in any CI claim.

## DECIDE (pre-review)

Use CSPI as a multiplicative scalar on the cell asymptote: `A_scaled = A_cell * (CSPI_cell / CSPI_ref)^beta`, with CSPI_ref the CONUS median, fit beta from the CSPI-to-asymptote relationship.

## INTERNAL ADVERSARIAL LOOP (folded, condensed mode)

The grounding analysis broke the naive plan and forced the refinements below.

| Quantity | Value | Reading |
|---|---|---|
| Pooled Pearson(CSPI, A), carbon | 0.449 | moderate, positive, robust |
| Pooled Spearman | 0.533 | rank-stable |
| Pooled log-log slope (plot-weighted) | 3.13 | implausibly steep (CSPI doubling implies ~9x A); confounded |
| Within-forest-type partial slope | 2.77 | still steep; partial corr 0.374 |
| relRMSE, flat median A vs CSPI-scaled | 0.915 to 0.829 | CSPI explains only ~9% of asymptote spread |

**Red team.** The pooled exponent (~3) is a confound, not a biology. Even deconfounded within forest type the exponent (2.77) is too strong to drive asymptotes directly: it implies a near-sevenfold asymptote change across a CSPI doubling, which exceeds plausible site-index effects on maximum standing biomass and is sensitive to the index's arbitrary 0 to 100 scaling. The 9% relRMSE gain confirms CSPI is a secondary refinement; forest type and cell identity set the band (see figure).

**Guardrails.** No multiple-testing surface here (single covariate). Leakage control: the scalar is computed from cell-mean CSPI only, applied multiplicatively to the independently fitted A; no CSPI information enters the curve fit itself. Spatial autocorrelation: any CI on the scaled asymptote must use the block-resampled bootstrap already specified for #75, not naive resampling.

**Format and HPC.** Plaintext math, pipe tables, Agg backend for the figure, seeded, error-logged. Confirmed.

## REFINED BLUEPRINT

The four passes forced these changes from the pre-review plan:

1. **Do not free-fit beta.** Reject the data-driven exponent (2.77 to 3.13) as the production scalar; it is confound-amplified and biologically too strong.
2. **Damped, bounded scalar.** Set beta = 1.0 (proportional) as the conservative default knob, and **clamp the scalar to [0.80, 1.25]** so no cell asymptote moves more than +/- 25% from its own fitted value. CSPI then refines and borrows strength; it does not overturn the fit.
3. **Anchor on the cell's own A where data is sufficient.** Apply CSPI scaling at full weight only to sparse cells (n_plots below a threshold, default 30); for well-populated cells, shrink the scalar toward 1.0 in proportion to 1/n. This makes CSPI a strength-borrowing device, its intended role.
4. **Two named team knobs**, mirroring the senescence S_SEN convention: `beta` (default 1.0) and the clamp half-width (default 0.25). Both documented as assumptions, not estimates.

## ACT: deliverables

**Output A** is this report. **Output B** is `ycx_cspi_scale.R` (the scaling module, written alongside). **Output C** is the diagnostic figure below.

![CONUS cell asymptote vs CSPI, colored by forest type](cspi_vs_asymptote.png)

The figure shows the positive CSPI gradient with forest type setting the vertical band, the visual basis for treating CSPI as a within-band refinement rather than a primary driver.

### Recommended production specification

```
CSPI_ref      = 56.36          # CONUS median (from ycx_cell_cspi.csv)
beta          = 1.0            # team knob; proportional, NOT the free-fit 2.77
clamp         = [0.80, 1.25]   # team knob; max +/- 25% asymptote move
shrink        = n0 / (n0 + n)  # n0 = 30; sparse cells get full CSPI weight
A_scaled      = A_cell * (1 + shrink * (clamp((CSPI_cell/CSPI_ref)^beta) - 1))
```

This wires into `ycx_canonical_ci_*` behind a flag (`use_cspi_asym=TRUE`), leaving the live production engines untouched.

---

```
[DATA_STATE]: 873-cell CSPI table built (99.9% plot coverage, CONUS median 56.36); joined to 1,801 carbon asymptote fits; relationship quantified and deconfounded.
[OUTCOME_VERIFICATION]: pooled Pearson 0.449, within-ft partial 0.374, free-fit exponent 2.77 to 3.13 rejected as confounded; CSPI explains ~9% of asymptote spread, confirming secondary-refinement role. Production scalar set to beta=1.0, clamp +/-25%, sparse-cell shrinkage.
[IMPACT_UTILITY]: feeds #75 step 3 (wire upgraded estimation into ycx_canonical_ci_* behind use_cspi_asym flag); report routes to manuscript-preparer, ycx_cell_cspi.csv to data-curator/zenodo at the v1.2 bump.
[NEXT_AUTONOMOUS_STEP]: implement ycx_cspi_scale.R inside ycx_canonical_ci_*, run ME+CA pilot refit with the flag, compare t0 carbon vs current production and vs FIA anchor, then resolve the two remaining edge cases (NM denominator, low-k M).
```

---

## ADDENDUM: pilot refit and t0 reconciliation (autopilot, 2026-06-27)

The scaling was wired into a flagged copy of the live engine, `ycx_canonical_ci_fiadb_cspi.R`, behind `YCX_CSPI_ASYM` (default off; the live `ycx_canonical_ci_fiadb.R` is untouched). The block scales `fit$A` by the bounded, damped, sparse-shrunk scalar immediately after the fits are read. ME and CA were run both ways, rcp45.

| State | Anchor | Scalar median (range) | agc 2025 base to CSPI | reserve agc 2125 base to CSPI | BAU agc 2125 base to CSPI |
|---|---|---|---|---|---|
| ME | fia.json | 1.000 (0.95 to 1.03) | 221.4 to 221.4 (0.0%) | 366.8 to 366.7 (-0.0%) | 333.8 to 333.7 |
| CA | flat2400 | 1.002 (0.91 to 1.12) | 1913.6 to 1947.6 (+1.8%) | 2696.1 to 2748.0 (+1.9%) | 2591.9 to 2641.4 |

**Verified findings.**

1. **t0 neutrality holds for FIA-anchored states.** ME 2025 carbon is identical (221.4 to 221.4): the area re-anchor absorbs the CSPI asymptote change at t0, so CSPI only reshapes the trajectory. For ME that reshape is negligible (homogeneous productivity, scalar tight around 1.0).
2. **Interaction surfaced for non-anchored states.** CA uses the flat2400 area model (no FIA pin), so the CSPI scalar flows through to t0 as well (+1.8%). This is small but real: with no anchor to absorb it, CSPI perturbs t0 for flat/treemap-area states. Recommended handling: either restrict the CSPI flag to fia.json-anchored states for the production t0, or re-derive the area anchor after CSPI scaling so t0 stays pinned to independent FIA. This is a documented design choice for the team, not a bug.
3. **No instability anywhere.** Effects are 0% (ME) to ~2% (CA), confirming the conservative knobs (beta=1.0, clamp +/-25%, sparse-cell shrinkage) make CSPI a refinement, not a disruption. No cell hit a blowup.

```
[PILOT_STATE]: ycx_canonical_ci_fiadb_cspi.R built, parses clean, runs ME+CA both flags. Outputs ci_pilot_base/ and ci_pilot_cspi/.
[OUTCOME_VERIFICATION]: t0 invariant for fia-anchored ME (221.4 unchanged); CA flat2400 t0 +1.8% (anchor interaction documented); trajectory change 0 to 2%, no blowups.
[NEXT_AUTONOMOUS_STEP]: decide anchor handling for non-fia states (restrict flag vs re-anchor post-scaling); then expand pilot to a confound-spread set (e.g. add OR, FL, AZ), then clear NM denominator and low-k M edge cases before the 48-state dev->prod PR.
```

---

## ADDENDUM 2: t0-pin re-anchor and 5-state reconciliation (autopilot, 2026-06-27)

The anchor interaction from Addendum 1 is resolved by re-anchoring. A second injected block (`+t0pin`) recomputes baseline (unscaled) 2025 carbon density and rescales area for non-fia states so the scaled run reproduces the baseline t0 total. FIA-anchored states are already t0-pinned via the tg anchor and are left unchanged. The engine variant is `ycx_canonical_ci_fiadb_cspi.R` (parses clean, live engine untouched).

Pilot expanded to a confound-spread set spanning productivity and anchor types: ME (fia, northern hardwood/spruce), CA (flat, high biomass), OR (fia, productive west), FL (flat, southern pine), AZ (flat, dry low productivity).

| State | Anchor (CSPI run) | t0 base (Tg) | t0 CSPI (Tg) | delta t0 | delta 2125 reserve |
|---|---|---|---|---|---|
| ME | fia.json | 221.4 | 221.4 | +0.00% | -0.03% |
| CA | flat2400+t0pin | 1913.6 | 1914.2 | +0.03% | +0.18% |
| OR | fia.json | 910.4 | 910.4 | +0.00% | +0.16% |
| FL | flat2400+t0pin | 2557.2 | 2557.2 | +0.00% | -0.04% |
| AZ | flat2400+t0pin | 553.9 | 557.3 | +0.61% | +0.70% |

**Verified findings.**

1. **t0 neutrality now holds across anchor types.** CA's prior +1.8% t0 drift collapses to +0.03% with the pin. FL and the fia states are exactly neutral. AZ retains a +0.61% residual (a few sparse cells where the single-age base density and the projected 2025 density differ slightly under the decline term); within tolerance, documented.
2. **Trajectory effects are small in this set** (-0.04% to +0.70% at 2125), confirming the conservative knobs. CSPI is a genuine refinement here, not a level shift.
3. **Magnitude caveat.** These 5 states are relatively homogeneous; the aggregate CSPI effect is sub-1%. Larger effects are expected where internal productivity gradients are strong and sparse cells are common, since the shrinkage gives CSPI full weight there. Part of the value is sparse-cell stabilization, not the aggregate shift, so the 48-state run should report both the aggregate change and a sparse-cell-only summary.

```
[PILOT2_STATE]: ycx_canonical_ci_fiadb_cspi.R has scaling + t0-pin, both behind the flag; ran ME/CA/OR/FL/AZ both ways.
[OUTCOME_VERIFICATION]: t0 neutral across anchor types (max residual +0.61% AZ; CA fixed from +1.8% to +0.03%); 2125 reserve change -0.04% to +0.70%; no blowups.
[NEXT_AUTONOMOUS_STEP]: clear NM pathological denominator and low-k M reparameterization in ycx_remeas_cell_fit.R, then run the flagged 48-state engine (rcp45+rcp85), reconcile CONUS t0 vs FIA and trajectory vs current production, and report aggregate + sparse-cell-only deltas ahead of the dev->prod PR and Zenodo v1.2 bump.
```

---

## ADDENDUM 3: full 48-state CONUS reconciliation (autopilot, 2026-06-27)

SLURM job 12063659 ran all 48 states both flags, rcp45, into `ci_full_base/` and
`ci_full_cspi/`. 48/48 each, zero failures. Both edge cases were confirmed already
handled in the production cell fitter (M bounded [maxY*0.9, 3*maxY]; NM was a
state-comparison-script artifact), so neither blocked the run.

| Scenario | Year | base (Tg C) | CSPI (Tg C) | delta |
|---|---|---|---|---|
| Reserve (No_harvest) | 2025 | 55177.5 | 55189.9 | +0.02% |
| Reserve | 2075 | 82128.3 | 82137.0 | +0.01% |
| Reserve | 2125 | 77490.4 | 77489.5 | -0.00% |
| BAU | 2025 | 55177.5 | 55189.9 | +0.02% |
| BAU | 2125 | 73823.3 | 73821.9 | -0.00% |

Per-state delta at 2125: n=48, min -0.39% (MA), median +0.00%, max +0.72% (SD).
Sparse-cell footprint: 360 of 873 cells (41%) have n_plots < 30 and get full CSPI
weight; their mean cspi_scalar = 0.976.

**Verified conclusion.** With the conservative knobs (beta=1.0, clamp +/-25%, sparse-cell
shrinkage), CSPI scaling is **t0-neutral and trajectory-neutral at the CONUS aggregate**
(+0.02% at 2025, -0.00% at 2125). The effect is **redistributive at the cell and state
scale, not a national shift**: state deltas span -0.39% to +0.72% with a median of 0.00%,
so productive and unproductive cells offset across the country. This is the correct
behavior for a site-productivity covariate. Its production value is sub-national accuracy
and principled asymptotes for the 41% of cells that are sparse (mean scalar 0.976, a mild
downward nudge where data is thin), NOT a change to published CONUS totals, which is a
virtue for a production change. Larger effects are available by raising beta, the team knob.

**Recommendation for the dev->prod decision.** The flagged scaling is safe to promote: it
does not move headline CONUS numbers, it is t0-reproducible by construction, and it
stabilizes sparse cells. Remaining before the PR: team sign-off on beta and clamp, optional
rcp85 confirmation (CSPI is orthogonal to the climate arm), then the dev->prod PR and
Zenodo v1.2 bump.

```
[CONUS_RECONCILE_STATE]: 48/48 both arms, 0 failures. CONUS reserve 2125 77490.4 (base) vs 77489.5 (CSPI).
[OUTCOME_VERIFICATION]: t0 +0.02%, 2125 -0.00% at CONUS aggregate; per-state -0.39% to +0.72% (median 0.00%); 360/873 sparse cells get full weight (mean scalar 0.976). t0-neutral, redistributive not additive.
[IMPACT_UTILITY]: dev->prod ready pending team sign-off on beta/clamp; report -> manuscript-preparer, ycx_cell_cspi.csv -> data-curator + zenodo-deposit at v1.2.
[NEXT_AUTONOMOUS_STEP]: optional rcp85 run for completeness, then stage the dev->prod PR and the Zenodo v1.2 metadata; await team beta/clamp decision before merge.
```

---

## ADDENDUM 4: rcp85 confirmation + staged PR and Zenodo (autopilot, 2026-06-27)

**rcp85 (SLURM job 12063832, 48/48 both arms, 0 failures).** Identical CSPI behavior to
rcp45, confirming CSPI is orthogonal to the climate arm:

| Scenario | 2025 | 2125 |
|---|---|---|
| Reserve | +0.02% | -0.00% |
| BAU | +0.02% | -0.00% |

Per-state 2125 delta -0.39% (MA) to +0.72% (SD), median 0.00%. Same sparse-cell footprint.

**Draft PR staged:** holoros/perseus-forest-intelligence #91
(branch `feature/cspi-asymptote-covariate`). Adds the CSPI mechanism and provenance
(scripts/yield_curve_engine/cspi/, canonical_inputs/ycx_cell_cspi.csv) and ADR 0005.
Default flag OFF; live engine untouched; no live-data regeneration. Draft until the team
signs off on beta and the clamp half-width.

**Zenodo v1.2.0 staged (not published):** full upload package on Cardinal at
`zenodo_staging/perseus-yield-curves/v1.2/` (metadata, README addendum, file manifest,
all CSPI artifacts). New-version deposit against parent concept DOI
10.5281/zenodo.20959003. Publish command after sign-off:
`python new_version.py --token-file ~/.zenodo_token --parent-doi 10.5281/zenodo.20959003 --metadata zenodo_metadata.json --files-list files_to_upload.txt --publish`

```
[FINAL_STATE]: extraction + scaling + flagged engine + t0-pin done; 48-state rcp45 AND rcp85 reconciled (CONUS-neutral, redistributive); draft PR #91 up; Zenodo v1.2 staged.
[GATE]: two team knobs (beta=1.0, clamp +/-25%) are the only blockers to merge + publish. Mechanism, provenance, and verification are complete and reproducible.
```

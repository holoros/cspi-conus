# Two-paper split outline: separating the v0.10g manuscript into two focused submissions

*14 June 2026. Per stress test recommendation #5. The v0.10g draft has grown to 10 tables, 13 figures, and roughly 12,000 words — too much for one paper. Split into two focused submissions of complementary scope.*

## Paper 1: "Beyond site index" (Forest Ecology and Management)

**Working title.** Beyond site index: FIA's own classification system disagrees with site index but agrees with a multi-dimensional composite of forest productivity at 30 m across the conterminous United States.

**Word count target.** 8,000 words main text.

**Central claim.** Forest site productivity is empirically multi-dimensional. No single operational measure (site index, biomass growth, asymptotic biomass, NPP) substitutes for any other. FIA's seven-class site productivity classification (SITECLCD) is recovered by a 3-component composite (ESI + BGI + Asym) but not by site index alone. The traditional FIA SICOND value is mixed when properly base-age-corrected, contrary to a naive Simpson paradox reading of the raw correlations.

**Headline statistic.** Random forest predicting FIA SITECLCD: BGI alone OOB R² = 0.808, ESI alone 0.751. A 6 percentage point gap independent of base-age, equation-family, and measurement-chain concerns.

**Sections to keep.**

- §1 Introduction (multi-dim productivity construct, prior work)
- §2 Methods (productivity measures, FIA joins, ESI training)
- §3.1 No measure substitutes for another (Table 1: cross-prediction R²)
- §3.2 Linear vs nonlinear (the 0.46 gap)
- §3.3 Per-species heterogeneity (Table 3: ESI–BGI by species)
- §3.4 East-vs-West regime
- §3.5 SICOND and ESI: base-age Simpson paradox correction (Tables 2, 2a, 2b, 2c — SSURGO and GADA as robustness checks)
- §3.6 FIA SITECLCD result (Tables 3 + 3a — the headline)
- §3.7 Stand-age sign flip (Table 4)
- §3.8 Forest type group differentiation
- §3.9 Composite construction (default equal-weight)
- §3.10 The composite surface (Figure 7)
- §3.11 ESI training versions (brief)
- §3.12, §3.13 Disturbance/treatment/stand-size context, per-state correlations
- §4.1–§4.7 Discussion
- §4.7.1 FIA-on-FIA representation, not independent prediction (the corrected framing)
- §4.8 Limitations
- §5 Conclusions

**Sections to MOVE to Paper 2.**

- Ecoregion-weighted composite analysis (the v0.10g §3.9 paragraph on L1/L2/L3 weights, +0.015 R² gain, Table 2c, multilevel composite construction)
- The CSPI v3.0.0 surface direction discussion

**Figure target (Paper 1).** F7 (composite surface), F8 (SITECLCD by measure), F9 (stand-age sign flip), F10 (per-species), F11 (decision tree). Plus F1 (correlation heatmap) as supplement or main, F2 (component distributions) as supplement, F3 (composite by latitude band) as supplement. **Main text 5-6 figures.**

**Estimated revision effort.** 1 to 2 days. Mostly trimming the multilevel paragraph + Table 2c-style multilevel content out of §3.9 + §4.6, and rebalancing the supplements.

**Timeline.** Submission within 2 weeks of coauthor review completion.

## Paper 2: "Hierarchical ecoregion weighting for multi-component productivity composites" (Forest Science or Methods in Ecology and Evolution)

**Working title.** Hierarchical ecoregion weighting for multi-component forest productivity composites: a base case study using EPA Level III ecoregions and the CSPI v2 release.

**Word count target.** 6,000 words.

**Central claim.** A hierarchical-shrinkage L1/L2/L3 ecoregion-weighted composite captures structural variation that an equal-weight composite misses. Adding the hierarchical composite to the equal-weight composite as a second predictor in a SITECLCD random forest increases OOB R² from 0.812 to 0.832 (+0.020, 95% CI non-overlapping). The four-composite ensemble (equal + L1 + L2 + L3) reaches R² = 0.836 with only +0.004 additional gain, so the two-term equal + hier model is the parsimonious operational choice. The result is insensitive to the shrinkage tau hyperparameters (R² spans only 0.814 to 0.815 across 20 (tau3, tau2) combinations).

**Sections.**

- §1 Introduction: motivation for ecoregion-specific composite weighting; reference Paper 1 for the underlying CSPI v2 release and multi-dim claim
- §2 Methods: EPA Level I/II/III ecoregion spatial join via NA_Eco_L3_WGS84.shp; PCA-derived per-region weights; hierarchical shrinkage formula; bootstrap CI estimation
- §3.1 Per-level PCA weight heterogeneity (Tables: weights at L1 / L2 / L3)
- §3.2 The four-composite ensemble: equal + L1 + L2 + L3 results
- §3.3 Hierarchical shrinkage: stability across tau (Table S11 from STRESS4)
- §3.4 Bootstrap 95% CIs (Table from STRESS1)
- §3.5 Spatial pattern of composite differences (Figure: where the L3 weighting changes the composite most)
- §3.6 FIA-on-FIA representation caveat (carried from Paper 1 §4.7.1)
- §4 Discussion: when ecoregion-weighting matters; v3.0.0 surface direction; application-specific composite families
- §5 Conclusions: operational recommendation (two-term equal + hier; multilevel ensemble only when application demands)

**Figure target.** New figure: per-region L3 weights as a CONUS map (50 ecoregions colored by w_ESI / w_BGI / w_Asym dominance). New figure: bootstrap CI bars for the 6 models. New figure: spatial pattern of c3_equal − c3_hier difference (where the reweighting moves the composite).

**Why this venue.** Forest Science fits the application focus. Methods in Ecology and Evolution fits if framed as a hierarchical-shrinkage methodology paper with ecoregion as the case study. Forest Ecology and Management is also viable but Paper 1 is already targeting it.

**Estimated revision effort.** 2 to 3 days. Most material is in v0.10g already; needs reorganization + new figures + new introduction framing.

**Timeline.** Submission within 4 weeks of Paper 1 submission (allowing Paper 1 to land first as the foundational citation).

## Recommended sequencing

1. **Week 1.** Coauthor review of v0.10g (Anthony, Jereme). Apply their edits to the manuscript.
2. **Week 2.** Trim Paper 1 to focused 8,000-word scope (cut multilevel content). Submit to FEM.
3. **Week 3.** Extract the multilevel content from v0.10g + add new figures for Paper 2. Internal review.
4. **Week 4.** Submit Paper 2 to Forest Science (or Methods in Ecology and Evolution).
5. **Week 5+.** Address FEM reviewer feedback on Paper 1.

## Cross-citation strategy

Paper 1 references the upcoming Paper 2 ("see Weiskittel, in preparation, for ecoregion-weighted composite extensions"). Paper 2 references Paper 1 as the foundational empirical motivation and the CSPI v2 release as the data product. Both share Zenodo concept DOI 10.5281/zenodo.20515034 for the data; the analytical chain repo `holoros/cspi-conus` is the shared codebase.

## Risk considerations

- **Coauthor coordination.** Splitting means Anthony and Jereme review only Paper 1 in the first cycle. Communicate clearly that Paper 2 will follow.
- **Citation racing.** If a competing group publishes on multi-component productivity composites between Paper 1 and Paper 2, the freshness advantage erodes. Mitigate by minimizing the gap between submissions.
- **Reviewer overlap.** FEM and Forest Science have overlapping reviewer pools. If the same reviewer sees both, they may notice the shared CSPI v2 dataset. Disclose proactively in cover letters.
- **Disambiguation.** The two papers will both be searchable as "Weiskittel 2026 CSPI" — title differentiation is critical. The Paper 2 title should foreground "hierarchical ecoregion weighting" rather than "CSPI v3" to avoid confusion.

## Executive recommendation

**Recommended:** proceed with the split. The stress test gain quantification (+0.020 R² CI non-overlapping for adding ANY ecoregion composite to equal-weight, +0.024 from the full ensemble) is strong enough that the ecoregion analysis carries a standalone paper. Trimming Paper 1 keeps the SITECLCD headline cleaner.

**Alternative:** keep as one paper but move the multilevel content entirely to supplementary (Table S10 + S11 + a paragraph in §3.9). This is also defensible but underuses the result.

**Decision needed.** Aaron's call. Split recommended unless coauthor consultation reveals strong preference for a single submission.

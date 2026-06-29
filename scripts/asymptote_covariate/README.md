# CSPI yield-curve asymptote covariate

Applies CSPI as a spatial-raster covariate scaling CONUS yield-curve cell asymptotes (ft x ecoregion). Signed off 2026-06-29 (ADR 0005): beta=1.5, clamp [0.60,1.60]; A_scaled = A*clamp((CSPI/REF)^beta), REF=56.36.

Deep out-of-sample assessment (docs/20260628_cspi_deep_assessment.md): CSPI +8.4% held-out skill for the cell asymptote vs ClimateNA SI +0.9% and latitude +3.7%, independent of SI (partial r +0.41). A uniform asymptote scalar cancels in the t0-anchored reserve trajectory, so CSPI acts on the spatial/absolute density layer. All FIA true coords stay server-side; only cell aggregates here.

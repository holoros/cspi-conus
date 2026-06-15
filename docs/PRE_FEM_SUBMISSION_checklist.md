# Pre-FEM-submission checklist

*v0.10g+ as of 14 June 2026. Items between current state and clicking the FEM submit button. Roughly two weeks of calendar time at the current cadence.*

## Critical path (must complete before submission)

### Week 1: Coauthor review

- [ ] Send `outreach/04_coauthor_review_email.md` to **Anthony D'Amato** (anthony.damato@maine.edu) and **Jereme Frank** (jereme.frank@maine.edu). One-week review window.
- [ ] Anthony review: SFR director perspective on the SITECLCD framing; coauthor line decision.
- [ ] Jereme review: CFRU operational perspective on the SICOND-vs-ESI swap calibration shift bound (5-10% claim in §4.3).
- [ ] Track responses in `outreach/05_coauthor_responses.md`.

### Week 2: Apply edits + executive decisions

- [ ] **Paper split decision** (Aaron's executive call). Per `docs/PAPER_SPLIT_outline.md`: keep as one paper with multilevel as supplements, OR split into Paper 1 (FEM) + Paper 2 (Forest Science or MEE)?
- [ ] If split: trim multilevel content from v0.10g+ to extract Paper 2 source. Reduces Paper 1 to ~8,000 words.
- [ ] If keep: ensure Tables 2c and the multilevel §3.9 paragraph are clear in their representation framing.
- [ ] Apply Anthony and Jereme edits.
- [ ] Re-export combined FEM docx after final edits.

## Polish items (high value, can run in parallel with coauthor review)

- [ ] **Verify the 3 placeholder references** are real and properly cited:
  - Westfall et al. (2023) NSVB methodology — confirm DOI
  - Modeling regional forest site productivity 2024 — confirm full citation
  - Smith et al. (2024) Site productivity and forest carbon stocks — confirm full citation
- [ ] **Citation breadth final scan** for 1-2 more recent (2024-2026) multi-metric productivity refs if available
- [ ] **Verify Tables 2a, 2b, 2c, 3a are properly numbered** in the docx (pandoc renders them as part of the main flow; check sequential numbering)
- [ ] **Figure caption polish**: confirm F11 decision tree caption is concise and Aaron-style
- [ ] **Cover letter polish**: confirm the 3-paragraph cover letter at the top of `submission_FEM_combined/_combined.md` reflects v0.10g+ findings (currently still has the v0.5 framing references in places)

## Pre-flight technical checks

- [ ] **FEM Article Type:** confirm "Research Article" is the right submission category (not "Methods", not "Review")
- [ ] **Word count check:** target 8,000-10,000 words for FEM. Current v0.10g+ is roughly 12,000 words. Trim or split.
- [ ] **Figure resolution check:** verify F7, F8, F9, F10, F11 are at 300+ DPI in the docx (some PNG sources may be 150 DPI)
- [ ] **Reference manager check:** Aaron may want to convert the markdown references to a proper reference manager (Zotero, EndNote, Mendeley) export before submission
- [ ] **Plagiarism / iThenticate check:** UMaine or FEM may auto-run this. Pre-check via Aaron's UMaine account
- [ ] **CRediT statement** is present in §5.5 — confirm correct
- [ ] **Competing interests declaration** in §5.6 — confirm correct
- [ ] **AI declaration** in §5.7 — confirm Aaron's preferred wording. Current text: "During the preparation of this work the author used Claude (Anthropic) for structural organization, consistency checking, and code drafting in the analytical pipeline. The author reviewed and edited all output and takes full responsibility for the content of the publication."

## Submission day

- [ ] Final FEM submission package: cover letter + manuscript docx + supplementary materials + figures (separate PDFs if FEM requests)
- [ ] Submit via Elsevier Editorial System for FEM
- [ ] Save the submission tracking number (e.g., FORECO-D-26-NNNNN)
- [ ] Update `holoros/cspi-conus` `NEWS.md` with submission date and tracking number
- [ ] Update Zenodo deposit metadata (both 20693106 and 20663652) with "in revision at Forest Ecology and Management, manuscript ID FORECO-D-26-NNNNN" in the description

## Post-submission day

- [ ] Notify UMaine Communications of submission (for press release timing)
- [ ] Notify CRSF associate directors
- [ ] Coordinate social media release window with Comms (LinkedIn variant B is ready in `outreach/02_social_posts.md`)
- [ ] **Hold all external communications** until at least the in-press notification

## v2.0.1 deposit (concurrent or after submission)

- [ ] Poll Cardinal until `v7_qrf` array completes (currently AssocGrpJobsLimit-blocked, ~36 hrs out at last check)
- [ ] Once `CSPI_V7_CONUS_30m_uncertainty.tif` lands, fire `new_version.py` on the v2.0.0 record with the new file
- [ ] Bundle Action 2 metadata refresh into the v2.0.1 release: add `hasPart` link to analytical chain DOI `10.5281/zenodo.20693106`; update source URL to `holoros/cspi-conus`
- [ ] Backfill the v2.0.1 DOI into the manuscript

## v3.0.0 future track (post-submission)

After FEM review feedback returns (typically 6-12 weeks):

- [ ] Address reviewer comments; resubmit if revisions
- [ ] If split: write Paper 2 introduction + new figures + Methods section
- [ ] Submit Paper 2 to Forest Science or Methods in Ecology and Evolution within 4 weeks of Paper 1
- [ ] Begin v3.0.0 surface family build on Cardinal (30m wall-to-wall composite + components + per-ecoregion uncertainty)
- [ ] FIA-FS engagement on the SITECLCD finding (Q4 2026)

## Risk reminders

- **The SICOND Simpson paradox correction is logged transparently in NEWS.md.** If a reviewer raises it as a flag, the response is: this was caught by our own pre-submission stress test and the corrected reading is reported in the manuscript. The transparency is the strongest defense.
- **FIA-on-FIA representation limit is explicit in §4.7.1.** If a reviewer asks for independent validation, the response is: the SITECLCD recovery is the headline; independent prediction validation is planned for v3.0.0 (named in §4.7.1).
- **The manuscript scope (12,000 words) is on the edge for FEM.** If a reviewer requests trimming, the paper-split path documented in `docs/PAPER_SPLIT_outline.md` is the prepared response.

## What's NOT needed before submission

- v2.0.1 Zenodo release (v2.0.0 is fine for the manuscript citation; v2.0.1 can ride in revision)
- v3.0.0 work (parallel track, not blocking)
- Outreach package release (coordinated with FEM in-press)
- Press release (held until in-press)

---

*Three-week timeline from today's v0.10g+ state to FEM submission. Coauthor review is the longest pole; everything else is polish that can run in parallel.*

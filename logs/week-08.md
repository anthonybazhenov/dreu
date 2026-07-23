# Week 8

**Dates:** 07-20 to 07-26

## Goals

- Fold Cellpose and the new single-bee U-Net into every model-comparison figure and the results table.
- Add Variation of Information (VOI) to the metric suite and regenerate all results through one evaluator.
- Build the failure-mode figures (splits/merges highlighted) and the whole-swarm comparisons.
- Rebuild the deck for the August 3 lab review.

## Approach and Implementation

I rewrote the evaluation entry points (`evaluate_all.py` for F1/AP, `evaluate_voi.py` for VOI) and a shared model registry (`comparison_core.py`), then regenerated every method's results through the identical matcher so the table is fully consistent. I built a napari failure-mode explorer (`failure_explorer.py`) that auto-ranks the worst splits and merges and renders per-cube panels (green = true positive, orange = split, purple = merge, red = FP, blue = FN), and generated the head-to-head comparison figures across all five methods. I also segmented the large hanging-swarm volumes and made the whole-swarm 3D renders, then rebuilt the presentation deck (`HoneybeeSegmentationOverview.pptx`) around the full story.

## Results

- **The new single-bee U-Net wins on both F1 and VOI**: F1@0.5 = **0.746**, F1@0.75 = **0.539**, VOI-total = **1.414** — versus fine-tuned StarDist (0.407), Cellpose (0.318), and watershed (0.249). It nearly eliminates the splitting failure (4 splits / 4 merges, vs StarDist's 127 / 2) and is the only model to clear IoU 0.75.
- Full head-to-head results table, failure-mode figures for all six cubes, IoU-distribution and metrics-bar charts, and whole-swarm 3D renders all generated. On the dense `ninaswarm` volume (111 ground-truth bees) the U-Net reached F1@0.5 = 0.65.
- Deck rebuilt and ready to present at the August 3 review.

## Notes

- Important caveat I'm tracking honestly: the U-Net and Cellpose are currently scored on only **4 of the 6 cubes** (anna, james, nina1, pedro), because the raw images for `caitleen` and `nina2` are missing, while StarDist and watershed use all 6. So "0.746 beats 0.407" is a 4-cube-vs-6-cube comparison — closing that gap by recovering the two missing cubes is the top priority before claiming the win in the paper.
- This week is still in progress (logging through 07-22); remaining tasks are restoring the two cubes, extending the whole-swarm comparison to the U-Net and watershed, and adding a held-out test set beyond LOOCV.

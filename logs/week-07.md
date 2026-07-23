# Week 7

**Dates:** 07-13 to 07-19

## Goals

- Build the k-fold evaluation scaffolding so the new U-Net drops into the same LOOCV metrics as the baselines.
- Start producing publication-quality figures for the paper and an upcoming lab review.
- Assemble a first version of the results deck to present the baseline story.

## Approach and Implementation

I wrote the fold-evaluation scaffolding for `swarmseg` (`evaluation/evaluate_folds.py`) plus k-fold benchmark configs, so the U-Net can be scored under the identical 6-fold LOOCV protocol and matcher as StarDist, Cellpose, and watershed. I also registered the project with a `.leshyi-project.toml` manifest so metric definitions and file locations are declared in one place.

On the figures side, I wrote the first generators — an intro figure (`make_intro_figure.py`) and a base-vs-fine-tuned StarDist comparison (`make_base_vs_finetuned_figure.py`) — and rendered a first ~8-slide deck telling the baseline story: the problem and dataset, the instance-level evaluation approach, and the StarDist results with the split failure mode. Danielle and I set an internal **August 3 lab review** as the target to present against, which is driving the figure and deck work.

## Results

- Evaluation scaffolding in place: the new U-Net can now be benchmarked on the same LOOCV metrics as every baseline.
- First real paper figures generated (intro, base-vs-fine-tuned).
- First ~8-slide deck assembled around the baseline results. At this point the best-scoring model is still fine-tuned StarDist (F1@0.5 = 0.407); the U-Net is built but not yet folded into the results table.

## Notes

- The Aug 3 review is a useful forcing function — building the deck surfaced which figures don't exist yet (failure-mode panels, a full head-to-head), which sets next week's priorities.

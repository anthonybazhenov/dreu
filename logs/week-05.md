# Week 5

**Dates:** 06-29 to 07-05

## Goals

- Run the full StarDist benchmark: base and fine-tuned, each with and without a hyperparameter sweep, under 6-fold leave-one-out cross-validation (LOOCV).
- Add a classical (non-ML) watershed baseline for an honest floor.
- Score everything through the shared instance matcher and diagnose the dominant failure mode.
- Commit to a concrete direction for the new model.

## Approach and Implementation

This was the heaviest build week. I wrote the LOOCV + sweep harness (`stardist_loocv.py`, `verify_cubes.py`) covering four StarDist variants — `base`, `base_sweep`, `finetuned`, `finetuned_sweep` — and distributed the training across three lab machines using GNU parallel job files (`jobs_lamarr.txt`, `jobs_hertz.txt`, `jobs_hopf.txt`) with a merge step. The sweep ranged over learning rate, number of rays, and patch size. I also built a `classical_watershed.py` baseline (anisotropy-aware EDT + h-maxima seeds + marker-controlled watershed), which doubles as the shared instance matcher, and a set of napari viewers (`multi_model_viewer.py`, `visualize_comparison.py`, `grid_viewer.py`) to inspect predictions against ground truth.

With all methods scored on the same matcher, I did the failure-mode analysis and wrote the "New Approaches" research plan: because StarDist's star-convex assumption breaks on touching, non-star-convex bees, the new model should be a boundary/affinity 3D U-Net with graph partitioning that has no shape prior, with SAM2 as a low-label backup and the watershed as the honest baseline.

## Results

- Full StarDist LOOCV benchmark complete. Fine-tuned StarDist reached **F1@0.5 = 0.407**, versus **0.007** for the zero-shot base model — roughly a 58× improvement from fine-tuning. The sweep winner (`lr3e-4_rays96_p44`) won all six folds.
- Classical watershed baseline scored **F1@0.5 = 0.249**.
- Diagnosis is clear and quantified: StarDist fails by **splitting** — 127 splits versus only 2 merges across the six folds — and its boundaries are loose, so F1 collapses to **0.015 at IoU 0.75**. This is exactly the failure the new model needs to target.
- Research direction locked: an affinity/boundary 3D U-Net with graph partitioning.

## Notes

- The split-vs-merge asymmetry (127 vs 2) is the whole story of the paper: it's not that StarDist misses bees, it's that it shatters single bees into pieces because of the shape prior.

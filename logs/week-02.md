# Week 2

**Dates:** 06-08 to 06-14

## Goals

- Resolve the data-format ambiguity from Week 1 — specifically the `(z, y, x)` axis convention that isn't recorded in the file metadata — so I can trust every downstream result.
- Get a first segmentation baseline running end-to-end on a real cube, even if the numbers are bad.
- Start with Cellpose since it's the more turnkey of the two baselines.

## Approach and Implementation

I wrote validation notebooks to nail down the axis convention and to reconvert the volumes into a consistent layout (`axis_fix_and_validate.ipynb` and `nrrd_axis_fix_reconvert.ipynb`). This mattered because loading a volume with the wrong axis order silently transposes it and makes every metric meaningless — so I verified visually that raw images and their instance labels line up after loading.

With the data plumbing sorted, I stood up a first Cellpose run and a small grid-search harness that logs each configuration to a simulation log (`simlog.json`), sweeping over pretrained model, diameter, and flow/cell-probability thresholds. I kept this early exploration in an `Archive/` notebook (`PelegLab-CellPoseOLD.ipynb`) since I expected to iterate heavily.

## Results

- Axis convention resolved and validated: raw volumes and instance labels now load in a consistent `(z, y, x)` layout, verified by overlaying them.
- First Cellpose baseline runs end-to-end on the cubes and produces instance predictions — rough, but the full load → predict → save loop works.
- Grid-search logging is in place so I can compare Cellpose configurations systematically next week rather than by hand.

## Notes

- The axis fix was worth the time: a mismatched convention would have quietly corrupted the labels, and it's exactly the kind of bug that wouldn't surface until much later.

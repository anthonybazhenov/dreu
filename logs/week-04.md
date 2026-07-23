# Week 4

**Dates:** 06-22 to 06-28

## Goals

- Bring up the second baseline, StarDist-3D, on the honeybee cubes.
- Design a fair, instance-level evaluation protocol that every method can be scored against identically.
- Write down the project's technical conventions so the work is reproducible.

## Approach and Implementation

I set up StarDist-3D in the TensorFlow environment and got it running on the cubes (`PelegLab-Stardist.ipynb`). StarDist predicts star-convex polyhedra per pixel and resolves instances by non-maximum suppression, which is a strong prior for blob-like cells but a questionable one for long, curved bees — I wanted to measure exactly how much that hurts.

The bigger piece of the week was the evaluation design. I decided against pixel IoU (it hides split and merge errors) in favor of *instance* matching: match predicted objects to ground-truth objects by 3D IoU, then count true positives, splits, merges, false positives, and false negatives, and report F1 and AP at IoU 0.5 and 0.75 plus mean matched IoU. The key discipline is that one identical matcher scores every method, so comparisons are fair. I also wrote the repo's `CLAUDE.md` to record the data format, the environment split, and this evaluation mandate, and set up two helper sub-agents (`seg-metrics` for computing metrics and `compare-figure` for GT-vs-model panels) to keep that work consistent.

## Results

- StarDist-3D runs end-to-end on the honeybee cubes and produces instance predictions.
- Evaluation protocol defined and documented: instance matching by 3D IoU, F1/AP at 0.5/0.75, explicit splits/merges, one shared matcher for all methods.
- Repo conventions written down (`CLAUDE.md`) and two evaluation/figure sub-agents scaffolded.

## Notes

- Committing to "score instances, not pixels" early is the decision I'm most confident in — it's what will make the eventual baseline-vs-ours comparison credible.

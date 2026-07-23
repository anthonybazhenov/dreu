# Week 3

**Dates:** 06-15 to 06-21

## Goals

- Run a proper, staged hyperparameter search for Cellpose instead of the ad-hoc grid from last week.
- Compare Cellpose backbones (Cellpose-SAM and Cellpose-DINO) on the honeybee cubes.
- Start thinking about the overall research direction — what a "new model" that beats these baselines would actually look like.

## Approach and Implementation

I built a two-stage hyperparameter search for Cellpose. Stage 1 sweeps the pretrained backbone, diameter, and thresholds; stage 2 refines the learning rate on the best candidates. Both stages checkpoint to JSON (`stage1_checkpoint.json`, `stage2_checkpoint.json`, `hyperparameter_search_results.json`) so long runs are resumable. I moved the working notebook to `PelegLab-CellPose.ipynb` and ran both Cellpose-SAM (`cpsam`) and Cellpose-DINO (`cpdino`) backbones. At this stage I scored models by Variation of Information (VOI) per fold, since I hadn't yet built the unified instance-matching evaluator.

Alongside the experiments, I started drafting the research plan with Danielle — framing whole bees as elongated, curved, touching shapes, and sketching candidate approaches for the eventual new model (boundary/affinity U-Net with graph partitioning, foundation-model options like SAM2, and a classical watershed as an honest baseline).

## Results

- Cellpose hyperparameter search runs staged and resumable; best Cellpose-SAM configuration reached a mean VOI around 1.4 across folds.
- Confirmed that off-the-shelf Cellpose, even tuned, over-fragments the dense bee clusters — useful evidence that a purpose-built model is warranted.
- First written research direction: the new model should target the *splitting* failure mode directly rather than relying on a shape prior.

## Notes

- VOI was a convenient early metric, but it's a pixel-partition score and hides instance-level split/merge structure — I'll need a real instance-matching evaluator before any cross-model comparison is trustworthy.

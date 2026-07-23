# Week 6

**Dates:** 07-06 to 07-12

## Goals

- Build the new model as a clean, reusable package rather than notebook code.
- Get a 3D U-Net training and doing full-volume inference on the honeybee cubes.
- Support more than one way of turning the network output into instances, so I can compare strategies.

## Approach and Implementation

I trained and helped build `swarmseg`, a config-driven 3D U-Net package, consolidating and adapting pieces of existing lab code (`swarm-segmentation-unet`, `swarm-segmentation-seeds`, `swarm-nematics`) into one clean codebase with a proper README, TorchIO augmentation, and Weights & Biases logging. The model is a flexible-depth `UNet3D` (`models/unet3d.py`), and I wrote patch-and-bump tiling (`core/patching.py`) so it can run inference on full volumes that don't fit in memory.

Rather than commit to a single instance-extraction strategy up front, we implemented two inference paths: a **mask** path (threshold the foreground probability, then connected components) and a **seed** path (local-maxima seeded region growing). This lets me benchmark both against the same evaluator later. I want to be precise here for the record: the model that actually trains and runs is a foreground-mask U-Net (BCE loss), and I also scaffolded the affinity/graph-partition variant from the Week 5 plan as the more ambitious direction; the mask U-Net is what I could get training cleanly first.

## Results

- `swarmseg` package built end-to-end: config-driven training, full-volume tiled inference, checkpointing, and experiment logging.
- Two working inference paths (mask and seed) so the instance-extraction choice becomes an experiment rather than an assumption.
- The new model now trains and produces predictions on the cubes — not yet benchmarked through the shared evaluator, which is next.

## Notes

- I'm being careful to distinguish what's *built and training* (mask U-Net) from what's *planned* (affinity + graph partition). I don't want the log to overstate the method before the affinity version is actually trained and scored.

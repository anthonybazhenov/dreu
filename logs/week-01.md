**Student:** Anthony Bazhenov  
**Mentor:** Danielle Chase  

# Week 1

**Dates:** 06-01 to 06-07

## Goals

- Start the internship on-site at the Peleg Lab (BioFrontiers Institute, CU Boulder) and meet with my mentor Danielle Chase and PI Orit Peleg to agree on the project scope.
- Understand the research question: 3D instance segmentation of honeybees in dense micro-CT volumes, with the deliverable being a paper plus publication-quality figures.
- Get access to the shared data and set up my development environment.
- Learn the dataset format well enough to start writing code against it.

## Approach and Implementation

I met with Danielle and Orit to lock in the project goal: fine-tune and benchmark the standard 3D segmentation baselines (StarDist and Cellpose), figure out where they fail on real honeybee data, and then build a model that beats them. We agreed the end product is a paper with figures that clearly show baseline failures versus our method.

I got access to the shared `PelegShared/` data drive and the labelled micro-CT cubes. Because StarDist runs on TensorFlow and Cellpose runs on PyTorch, I set up a split environment (a `CUBoulder` conda env) so I can switch between the two stacks cleanly. I spent time reading through the labelled volumes to understand the format: 3D instance masks stored as `uint16` where each integer voxel value is an instance ID and 0 is background, with axis order `(z, y, x)`. Importantly, the axis order and physical voxel spacing are *not* stored in the file metadata, so I noted that any code will have to assume the convention explicitly — a likely source of bugs.

## Results

- Project scope agreed with my mentor: beat StarDist/Cellpose baselines on 3D honeybee instance segmentation; deliverable is a paper with figures.
- Environment set up with separate TensorFlow (StarDist) and PyTorch (Cellpose) stacks under a shared conda env.
- I now understand the data format (`uint16`, `(z, y, x)`, integer = instance ID) and flagged the missing-metadata axis-order issue as something to handle carefully before trusting any results.
- This was mostly ramp-up and orientation; hands-on coding starts next week.

## Notes

- Open question for Danielle: the volumes don't store physical voxel spacing, so I'll need the correct z-anisotropy to make StarDist-3D and watershed behave — need to confirm the right value.

# DREU Research Log

**Student:** Anthony Bazhenov  
**Mentor:** Danielle Chase  
**Principal Investigator:** Orit Peleg  
**Institution:** BioFrontiers Institute, University of Colorado Boulder  

This repository contains my weekly research log for the Distributed Research Experiences for Undergraduates (DREU) program. It covers 10 weeks. All 10 log files are pre-created — I fill them in week by week.

## Research Project Description

**3D honeybee instance segmentation.** The goal is to accurately separate individual
bees in dense, touching clusters imaged by micro-CT. I first fine-tune and benchmark
existing 3D segmentation baselines (StarDist and Cellpose), diagnose where they fail,
and then build a new model that beats them. Segmentation is scored at the *instance*
level (3D IoU matching, not pixel IoU) via 6-fold leave-one-out cross-validation over
six labelled cubes, reporting F1/AP at IoU 0.5 and 0.75, splits/merges, and Variation
of Information. The deliverable is a paper with publication-quality figures showing
where the baselines fail and where our model succeeds.

## Relocation Status

Relocated in person to Boulder for the summer, working on-site with the Peleg Lab at
the BioFrontiers Institute, University of Colorado Boulder.

## Repository Structure

```
your-repo/
├── README.md          ← this file
├── TEMPLATE.md        ← reference template (do not edit)
├── EXAMPLE.md         ← filled-out example showing expected quality
└── logs/
    ├── week-01.md
    ├── week-02.md
    ├── ...
    └── week-10.md
```

## How to Submit Your Weekly Log

1. Open the current week's file in `logs/` (e.g., `logs/week-03.md`).
2. Fill in the **Dates** field and all three sections.
3. Commit and push your changes before the weekly deadline (Sunday 11:59 PM).

You do not need to create new files — all 10 are already in the repo waiting for you.

## Log Format Requirements

Each weekly log **must** follow this exact structure for automated validation to pass:

```markdown
# Week N

**Dates:** MM-DD to MM-DD

## Goals
...

## Approach and Implementation
...

## Results
...
```

The following fields are required and must appear exactly as written:

| Field | Example |
|---|---|
| H1 title | `# Week 3` |
| Dates line | `**Dates:** 06-16 to 06-22` |
| Section 1 | `## Goals` |
| Section 2 | `## Approach and Implementation` |
| Section 3 | `## Results` |

The first three sections are required and must each contain at least one non-empty line of content. A fourth section, `## Notes`, is optional — include it for questions, follow-up items, or anything else that doesn't fit the main sections.

See **EXAMPLE.md** for a complete filled-out sample.

## Checking Your Log

You can validate your own log at any time by running the check script locally:

```bash
python check_log.py logs/week-NN.md
```

Or push your changes — if a GitHub Action is configured in this repo, it will automatically validate your latest log and show a pass/fail status on your commit.

## Questions?

Contact the DREU program staff <dreu_staff@cra.org> if you have questions about the format or submission process.

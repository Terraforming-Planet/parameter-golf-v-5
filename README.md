# Parameter Golf V5

This repository is a **clean V5 workspace** for the OpenAI Parameter Golf challenge.

The goal is straightforward: improve compression performance while staying within the official challenge limits and keeping every result reproducible and reviewable.

## Official challenge constraints (plain language)

- Evaluation benchmark is the official FineWeb validation setup used by the challenge.
- The submission artifact must remain under **16,000,000 bytes** total.
- Record-track runs must be reproducible in **10 minutes on 8×H100**.
- Results must be evidence-based and reproducible.
- No fabricated numbers, no unsupported claims.

## Three tracks in this repository

### 1) Official submission path (primary)

This is the only path intended for challenge submissions:
- FineWeb-centered training/evaluation workflow
- Official-style submission packaging
- Reproducible logs and metadata

### 2) Carried-over reference baseline (context only)

A reference baseline from prior work is retained for continuity:
- `final_val_bpb = 1.22064591`
- seq4096-style run family

This baseline is a **reference point**, not a V5 claim and not a leaderboard claim.

### 3) Auxiliary V5 dataset research path (experimental)

The custom V5 dataset effort exists as a separate experimental track:
- auxiliary and research-only
- not the official benchmark path
- must remain clearly separated from official FineWeb submission work

## V5 objective

- Immediate objective: beat the carried-over reference region around **1.22064591** cleanly.
- Mid objective: establish a disciplined path to **sub-1.15**.
- Longer objective: pursue **sub-1.10** only with reproducible, compliant evidence.

## Repository layout

- `README.md` — repository scope, constraints, and operating model.
- `STATUS.md` — concise current status and targets.
- `docs/v5/` — V5 roadmap, design principles, and compliance notes.
- `records/` — run records and submission-style artifacts.
- `records/reference_baseline/` — carried-over baseline record (reference-only).
- `tools/` — helper scripts/tooling for clean reproducible workflows.
- `datasets/solutions_training_v5/` — auxiliary V5 dataset placeholders and schema.
- `archive/` — optional, only when historical material is truly needed.

## Working style

V5 intentionally favors:
- low-entropy process,
- one canonical source of truth per run,
- explicit compliance boundaries,
- short and reviewable iteration loops.


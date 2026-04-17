# V5 leaderboard-aligned official track

This note defines the **official-record-track path** for V5: stay on official FineWeb artifacts, avoid unreviewed mixing, and push the fastest serious route from ~1.22 toward <1.15 and then <1.10.

## Why public 8Planetterraforming datasets are auxiliary-only (for now)

- The public 8Planetterraforming datasets are useful for experiments and stress tests, but they are **not yet part of the main official record path**.
- Keeping the main track on official FineWeb data preserves comparability, cleaner compliance review, and simpler artifact accounting.
- We can evaluate auxiliary data in side tracks, then promote only after clear reproducible gains and policy alignment.

## Why seq4096-only tuning plateaued

- A seq4096-only strategy can improve quickly early, but it tends to saturate once easy optimization wins are exhausted.
- Leaderboard movement has come from architecture/optimizer motifs used together, not from sequence-length-only retuning.
- To move meaningfully under ~1.15, we need the merged proven stack rather than another weak seq4096-only pass.

## Why SP4096 + parallel residuals + recurrence is the next move

- **SP4096/SP8192** are the official merged-tokenizer motifs now repeatedly used in competitive runs.
- **Parallel residuals** improve optimization flow and throughput-quality tradeoff in this scale regime.
- **Depth recurrence** (selected layers, small repeat count) gives more effective depth without full parameter growth.
- **MuonEq-R style stronger matrix decay** (MUON_WD≈0.09) and **QK-Gain 5.0** are already proven in merged top entries.

## Exact run commands

A. SP4096 clean
```bash
RUN_ID=v5_sp4096_clean
DATA_PATH=./data/datasets/fineweb10B_sp4096
TOKENIZER_PATH=./data/tokenizers/fineweb_4096_bpe.model
VOCAB_SIZE=4096
VAL_LOSS_EVERY=0
TRAIN_LOG_EVERY=0
torchrun --standalone --nproc_per_node=1 train_gpt.py
```

B. SP4096 + parallel residuals + recurrence + MuonEq-R
```bash
RUN_ID=v5_sp4096_top10
DATA_PATH=./data/datasets/fineweb10B_sp4096
TOKENIZER_PATH=./data/tokenizers/fineweb_4096_bpe.model
VOCAB_SIZE=4096
NUM_LAYERS=9
MODEL_DIM=512
NUM_HEADS=8
NUM_KV_HEADS=4
MLP_MULT=2
PARALLEL_RESIDUAL=1
RECURRENCE_LAYERS=4,5
RECURRENCE_STEPS=2
MUON_WD=0.09
QK_GAIN_INIT=5.0
VAL_LOSS_EVERY=0
TRAIN_LOG_EVERY=0
torchrun --standalone --nproc_per_node=1 train_gpt.py
```

C. SP8192 + same stack
```bash
RUN_ID=v5_sp8192_top10
DATA_PATH=./data/datasets/fineweb10B_sp8192
TOKENIZER_PATH=./data/tokenizers/fineweb_8192_bpe.model
VOCAB_SIZE=8192
NUM_LAYERS=9
MODEL_DIM=512
NUM_HEADS=8
NUM_KV_HEADS=4
MLP_MULT=2
PARALLEL_RESIDUAL=1
RECURRENCE_LAYERS=4,5
RECURRENCE_STEPS=2
MUON_WD=0.09
QK_GAIN_INIT=5.0
VAL_LOSS_EVERY=0
TRAIN_LOG_EVERY=0
torchrun --standalone --nproc_per_node=1 train_gpt.py
```

## Guardrails

- Do not add custom dataset mixing to the official main path yet.
- Avoid risky/unclear TTT features in main-track runs.
- Keep run manifests, logs, and artifact accounting complete and auditable.

# Under-1.15 official-compatible track (v5)

## Baseline reference

- Current baseline reference is approximately **1.22098568 val_bpb** on the official FineWeb path.

## Why simple `TRAIN_SEQ_LEN=4096` alone is not enough

- Moving to sequence length 4096 by itself usually improves context usage, but it does not reliably close the full gap from ~1.221 to <1.15.
- To make a serious under-1.15 attempt while staying official-compatible, this track adds controlled architectural/optimization toggles behind environment flags (parallel residual, selective recurrence, Muon weight decay), while keeping the baseline unchanged when flags are off.

## Primary command

```bash
RUN_ID=v5_under115_a \
DATA_PATH=./data/datasets/fineweb10B_sp1024 \
TOKENIZER_PATH=./data/tokenizers/fineweb_1024_bpe.model \
VOCAB_SIZE=1024 \
TRAIN_SEQ_LEN=4096 \
ITERATIONS=6000 \
WARMUP_STEPS=30 \
MAX_WALLCLOCK_SECONDS=0 \
NUM_LAYERS=9 \
MODEL_DIM=512 \
NUM_HEADS=8 \
NUM_KV_HEADS=4 \
MLP_MULT=2 \
PARALLEL_RESIDUAL=1 \
RECURRENCE_LAYERS=4,5 \
RECURRENCE_STEPS=2 \
MUON_WD=0.09 \
QK_GAIN_INIT=5.0 \
VAL_LOSS_EVERY=0 \
TRAIN_LOG_EVERY=0 \
torchrun --standalone --nproc_per_node=1 train_gpt.py
```

## Fallback command

```bash
RUN_ID=v5_under115_b \
DATA_PATH=./data/datasets/fineweb10B_sp1024 \
TOKENIZER_PATH=./data/tokenizers/fineweb_1024_bpe.model \
VOCAB_SIZE=1024 \
TRAIN_SEQ_LEN=4096 \
ITERATIONS=6000 \
WARMUP_STEPS=30 \
MAX_WALLCLOCK_SECONDS=0 \
NUM_LAYERS=9 \
MODEL_DIM=512 \
NUM_HEADS=8 \
NUM_KV_HEADS=2 \
MLP_MULT=2 \
PARALLEL_RESIDUAL=1 \
RECURRENCE_LAYERS=4,5 \
RECURRENCE_STEPS=2 \
MUON_WD=0.09 \
QK_GAIN_INIT=5.0 \
VAL_LOSS_EVERY=0 \
TRAIN_LOG_EVERY=0 \
torchrun --standalone --nproc_per_node=1 train_gpt.py
```

## Risks (short note)

- Selective recurrence increases per-step compute and can increase runtime variance.
- Artifact-size risk is expected to stay low, because parameter count is unchanged and recurrence only affects forward-pass compute, not checkpoint tensor shapes.

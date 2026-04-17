# SP1024 top10 under-1.15 track (v5)

## Baseline carried over

- Current carried-over V5 baseline is approximately **1.22098568 val_bpb** on the official SP1024 FineWeb path.

## Why we are abandoning SP4096 for now

- The current SP4096 path is not stable enough in this repo right now, so the fastest reliable path is to keep the official, already-working SP1024 data pipeline unchanged and improve only model/training code.

## Exact run commands

A. First serious code-side run
```bash
RUN_ID=v5_sp1024_top10_a
DATA_PATH=./data/datasets/fineweb10B_sp1024
TOKENIZER_PATH=./data/tokenizers/fineweb_1024_bpe.model
VOCAB_SIZE=1024
TRAIN_SEQ_LEN=4096
ITERATIONS=6000
WARMUP_STEPS=30
MAX_WALLCLOCK_SECONDS=0
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

B. Fallback if A is unstable
```bash
RUN_ID=v5_sp1024_top10_b
DATA_PATH=./data/datasets/fineweb10B_sp1024
TOKENIZER_PATH=./data/tokenizers/fineweb_1024_bpe.model
VOCAB_SIZE=1024
TRAIN_SEQ_LEN=4096
ITERATIONS=6000
WARMUP_STEPS=30
MAX_WALLCLOCK_SECONDS=0
NUM_LAYERS=9
MODEL_DIM=512
NUM_HEADS=8
NUM_KV_HEADS=2
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

## Short risk note

- Artifact-size risk is low: parallel residual and mini depth recurrence change compute flow, not parameter tensor shapes; the main risk is runtime instability/variance, not checkpoint size inflation.

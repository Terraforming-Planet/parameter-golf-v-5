# V5 Design Principles

1. **One canonical state**
   - Keep one authoritative run state per experiment.
   - Avoid fragmented, contradictory tracking.

2. **Ask instead of guess**
   - Treat unknowns as unknowns.
   - Resolve missing context explicitly before execution.

3. **Short stepwise execution**
   - Prefer small, auditable steps over monolithic workflow changes.
   - Keep every change easy to review and rollback.

4. **Preserve exact strings / filenames / numbers**
   - Maintain literal fidelity for paths, IDs, metrics, and thresholds.
   - Reduce accidental drift from casual renaming or reformulation.

5. **Compress information instead of expanding it**
   - Favor symbolic and structured representations where possible.
   - Avoid unnecessary verbose expansions that increase entropy.


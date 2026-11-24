# octo-flywheel — the data flywheel over the octo personalization lever

> 🐙 **The code is glue. The moat is the data you accumulate.** *Open plumbing, private data.*

The personalization lever (rich profile + force-reference, see
[octo-personalization](https://github.com/greatwhitesharklab/octo-personalization)) is copyable. So is
**this** flywheel plumbing — anyone can write capture/write-back/consolidation. **What is NOT copyable is
the per-customer data you run through it**: accumulated memory + interaction history + outcome-weighted
salience, compounding with every interaction. That data stays yours; this repo opens the plumbing.

## The flywheel

```
interact (lever) -> capture (turn + referenced facts + outcome) -> write-back (new facts, overwrite)
   -> consolidate (refresh profile / prune / salience) -> feedback (outcome-weighted salience) -> next ↺
```

Each turn grows the customer's memory and learns which facts/approaches correlate with good outcomes,
so the next reply is more personalized. **More use → richer memory → better personalization → more
engagement → more data.** The compounding is the value; the code is just pipes.

## What's in the box
- `octo_flywheel/capture.py` — append-only per-user interaction log (auditable).
- `octo_flywheel/writeback.py` — fact extraction (placeholder) + `(entity, attribute)` overwrite write-back.
- `octo_flywheel/consolidate.py` — profile refresh / prune / salient-value ranking.
- `octo_flywheel/feedback.py` — **outcome-weighted salience** (plain statistics, not a learned mechanism).
- `octo_flywheel/loop.py` — `Flywheel.step()` orchestrates the loop; `forget_user()` = right to be forgotten.
- `demo/flywheel_demo.py` — compounding demo: scripted multi-turn reveals → referenced-fact count grows.
- `tests/` — offline tests (mock backend, no model/GPU).

## Honest scope
- **This is product plumbing (glue), not a research mechanism.** The extractors are placeholders
  (a real product uses an LLM extractor); the outcome signal is **simulated** here (a real product fills
  it with actual customer behavior: replied / engaged / repaid).
- The "learning" is deliberately **plain outcome-weighted statistics** — the moat is the accumulated
  outcome data, not a clever algorithm.
- Synthetic demo only; no real customer data is distributed.

## Quickstart
```bash
pip install -e .            # requires octo-personalization (sibling or pip)
python -m pytest tests/ -q  # offline, no model
# compounding demo (download Qwen3.5-0.8B/4B first)
python demo/flywheel_demo.py --model /path/to/Qwen3.5-0.8B --device cpu
```
Expected: referenced-fact count rises from early turns to late turns (memory compounding).

## Relation
- 🐱 [huamao-memory-bench](https://github.com/greatwhitesharklab/huamao-memory-bench) — native K/V memory engine.
- 🐙 [octo-personalization](https://github.com/greatwhitesharklab/octo-personalization) — the lever + honest claims.
- 🐙 **octo-flywheel** (this) — the data plumbing over the lever.

## License
Apache-2.0. See [LICENSE](LICENSE).

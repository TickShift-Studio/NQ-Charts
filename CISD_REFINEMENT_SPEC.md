# CISD Detection — Refinement Spec (v2)

Builds on the core CISD you already coded and validated (trigger = OPEN/body-top of the first origin candle in the series; body close through it; bodies not wicks; 29,582 confirmed on the 7/22 chart). Do NOT change that core.

Add these three filters **in this order**, and **backtest after each one** so we can see its effect on hit rate before adding the next. Synthesized from 5 studied CISD sources (Traders_edu + TTrades); details in `CISD_MASTER_REFERENCE.md`.

---

## FILTER 1 — Manipulation / sweep precondition (add first)
A CISD only counts if price **swept liquidity before the confirming close.** No sweep → discard.

**Rule (bullish; mirror for bearish):**
- Before the body-close-above-origin-open that confirms the CISD, the origin low must have **traded below a prior liquidity level** — i.e., a "raid."
- Liquidity level = the more recent of: (a) the last minor swing low (a candle low with higher lows on both immediate sides), or (b) any marked level in the repo/feed (session low, PDL, premarket low, London/Asia low, midnight open).
- If the origin low did not take out any such level within a lookback of **N candles (default N = 50)** → the CISD is invalid, drop it.

**Params to expose:** `N` lookback; whether "liquidity" = swings only, marked levels only, or both (default: both).

**Log:** for every raw CISD, record `swept_liquidity: true/false` and which level. This alone should remove a lot of noise marks.

---

## FILTER 2 — V-shape quality filter (add second)
Real CISDs come from a **sharp V-shaped reversal**, not a slow consolidation that grinds through.

**Rule:**
- Count candles from the **origin extreme** (the lowest low for bullish) to the **confirming close**.
- If that count > **K (default K = 3)** → mark the CISD **low-quality / discard**. Swift recovery = intent; a long grind = range manipulation that usually fails.
- Optional stricter add later: require the recovery leg to be mostly one-sided (limited overlap between candles). Start with just the candle-count rule.

**Params:** `K` max candles (default 3).

**Log:** `recovery_candles: <int>`, `passed_vshape: true/false`.

---

## FILTER 3 — Resweep invalidation (add third)
A CISD is false if the extreme that created it gets **re-swept before the move plays out** — the valid CISD is the one off the final/deepest sweep. (Andre's CISD-3 case on the 7/22 chart.)

**Rule (bullish; mirror for bearish):**
- Track each confirmed CISD's **origin low** `L`.
- If, after confirmation and **before the target/draw is reached**, a later candle prints a low **below `L`** → invalidate that CISD.
- Then look for a **new CISD off the new (deeper) low** and apply Filters 1–2 to it.

**Params to pin (start with these defaults, tune on backtest):**
- Re-sweep trigger: **wick below `L`** (conservative default) vs. body-close below `L`. Default: wick.
- Invalidation window: until the **first projected target / draw** is hit. After the target is reached, later lows do NOT retroactively invalidate.

**Log:** `resweept: true/false`, `superseded_by: <new CISD id>`.

---

## Backtesting requirement (do this between each filter)
Run detection over a few hundred sessions and output a table:
- count of CISDs detected, and for each filter stage: how many pass, win rate, avg R (to the −2/−2.5 std-dev projection or defined target), max drawdown.
- Show me the numbers **before vs after** each filter so we can confirm each one actually improves quality, not just reduces count. Log every CISD with its filter flags so nothing is silently dropped.

## Do NOT add yet (parked — only after the above backtest well)
MSS/inversion pairing, TTrades candle-2/candle-3 HTF↔LTF fractal model, intra-candle CISD (ICCISD), reversal-vs-continuation classification, ranking, HTF-FVG/session filtering. Keep it simple; prove the three filters first.

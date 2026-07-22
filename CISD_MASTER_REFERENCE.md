# CISD — Master Reference (the "CISD teacher")

Living knowledge base built from studied CISD videos. Feeds BOTH the trading bot specs AND Andre's class. Sources listed at bottom.

---

## 1. Core definition
**CISD = Change in State of Delivery** = the **order block formation that occurs when trend changes** (bullish→bearish or bearish→bullish).

**What confirms it:** price reaches into an important level (FVG / old high / old low) OR sweeps liquidity, THEN **body-closes through the series of candles** that reached into that level. The level = the **OPENING PRICE of the FIRST candle** in that series.
- **Bullish:** after a sweep of a low, body-close **above** the open of the first DOWN-close candle → validates a bullish order block, flips to bullish.
- **Bearish:** after a sweep of a high, body-close **below** the open of the first UP-close candle → validates a bearish order block, flips to bearish.
- **Bodies, not wicks.** Series → use the FIRST candle's open. (Confirmed on Andre's 7/22 chart: trigger 29,582 = the open/body-top, not the 29,595 wick.)

## 2. Precondition — manipulation FIRST
CISD only counts **after a sweep / reach into a PD array.** No sweep → the close means nothing. The ideal is a **sharp V-shaped reversal** into the level and back through (see §5).

## 3. CISD vs MSS vs Inversion — three ways order flow shifts (TTrades v3)
| | Trigger | Timing | Use |
|---|---|---|---|
| **CISD** | body close through the **opening price** of the up/down-close series → **order block** | happens **FIRST / earliest** | earliest entry; can be premature |
| **MSS** | displacement + body close through the **swing high/low** | happens **LATER** | confirms trend; wait for retrace to premium/discount PD array |
| **Inversion (IFVG)** | opposing FVG gets fully closed through (prefer full close, not just 50%) | early, ~with CISD | alt confirmation on large candles |
- **CISD gives earlier entries than MSS** — its benefit. But earlier ≠ always better: CISD can put you in a loser where MSS would've kept you out (if MSS never confirms, trend never actually shifted).
- Pick what your eyes trust. TTrades personally prefers **order blocks (CISD)**. Backtest to find yours.

## 4. Fractal swing-confirmation model (TTrades v1) — the pairing
A trend shift is only valid when a **higher-timeframe candle closure** is paired with a **lower-timeframe CISD** inside it:
- **HTF "candle-2 closure"** = a candle sweeps the *previous* candle's high/low and closes back in (a swing point).
- **HTF "candle-3 closure"** = when there's no candle-2, candle-3 closes beyond the OPEN of candle-2.
- THEN drop inside that HTF candle → require a CISD (close through the series that made the extreme).
- **Both required.** HTF closure with no LTF CISD = invalid (wait/ignore). CISD with no HTF closure = meaningless. This filter saves losses on fake swings.

## 5. Quality filter — V-shape, not consolidation (TTrades v1 + v2)
- Ideal CISD = **aggressive V-shaped reversal**, closes through in **1–3 candles.** Swift = intent.
- If price **consolidates then closes over**, that closure is usually just a **manipulation of the range** → it fails and trades the other side. Not a real CISD.
- Rule of thumb: 2 candles down / 6 candles struggling back up = NOT a V-shape, don't trust it.

## 6. Why continuations/CISDs FAIL — three failure modes (TTrades v2)
1. **No V-shape (consolidation):** the close is a range manipulation. → Wait one candle: new continuation (trade it) or fail back across range (fade it).
2. **Forms while taking out a short-term high/target:** that's often where the opposing (HTF-wick) CISD forms against you. → Wait for a *further* continuation to confirm.
3. **A higher-timeframe target already got hit:** don't FOMO a continuation after a HTF objective or after 3+ candles of expansion — that's where a NEW phase / opposing CISD forms. Miss it, live to trade another day.
- Master takeaway: **frame entries around where the higher-timeframe WICK is forming**, not random continuations.

## 7. RESWEEP invalidation (Andre's refinement, corroborated by sources)
If the low (bullish) or high (bearish) that made a CISD gets **re-swept before the move plays out**, that CISD is false — the valid one is off the **final/deepest sweep** (not taken out again). Andre's test case: CISD 3 on his 7/22 indicator chart. Matches §2 (needs the final sweep) and TTrades v1's "first attempt failed, the re-sweep's close counted." Params to pin when coding: (a) re-sweep = wick vs body below origin; (b) window = only before the draw is reached.

## 8. Intra-candle CISD / ICCISD (TTrades v4) — ADVANCED, not base
A CISD **inside a single HTF candle**, used to mechanically define when that candle's **wick has finished forming** so you can trade the body. Reversal CISD confirms a swing; ICCISD confirms a continuation/wick. Ideal early in the candle so it can expand; if late but valid, wait for the next candle. Quote: *"If you don't have the patience to let the wick form, you become the wick."* → Park for a later, advanced class; not needed for base bot.

## 9. Full entry model (across sources)
**Manipulation → CISD → order block / FVG retest → projection targets.**
1. Manipulation: sweep of an old high/low or reach into a PD array, back into range.
2. CISD: body close through the series' opening price → also = order block.
3. Entry: on the CISD close, or retest of the OB/FVG, or the **0.5 / mean threshold** of the OB. Stop on the sweep extreme (manipulation high/low).
4. Targets: project the manipulation leg → **−2 to −2.5 std dev** (and −4/−4.5), stacked with liquidity (equal highs/lows, session H/L, HTF PD arrays).
5. Only take longs in **discount**, shorts in **premium** of the dealing range.
6. Confluence: SMT, continuation order blocks, AMD/Box setup.

## 10. Bot spec status
- Core CISD (open/body-top reference, bodies not wicks): coded & validated ✅
- Next add (in order, each backtested): (1) **manipulation/sweep precondition** (§2), (2) **V-shape quality filter** (§5), (3) **resweep invalidation** (§7). These three are what make the marks "clean."
- Keep parked: MSS/inversion pairing, fractal HTF-LTF model, ICCISD, classification/ranking — add only after the core + those 3 filters backtest well.

---

## Sources studied
1. `CHIK5oBRKiw` (Traders_edu, 15:44) — CISD PDF + AMD/Box entry model + std-dev projections. Established: opening-price-of-first-candle reference (validated bot).
2. `vn1RYjhJUnQ` (TTrades, 10:24) — CISD confirms swing points; candle-2/candle-3 fractal pairing; V-shape quality; failure→consolidation.
3. `PQiRV0JMhIQ` (TTrades, 29:36) — why continuations fail; 3 failure modes; frame entries around the HTF wick.
4. `E89d1HArbgM` (TTrades, 9:18) — MSS vs CISD vs Inversion comparison; CISD is earliest but can be premature.
5. `Vo2n47RjjMo` (TTrades, 14:06) — Intra-candle CISD (ICCISD), advanced wick-formation model.

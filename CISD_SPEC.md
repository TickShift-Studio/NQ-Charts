# CISD — Change in State of Delivery (spec for the trading bot)

This is the exact definition to implement. Author: Andre (AnixWallo). Do not substitute a generic/textbook version — use this one.

**Look at `CISD_chart.png` in this same folder** — it's Andre's annotated NQ 1-min chart marking two real CISDs (CISD 1 ~09:50, CISD 2 ~10:25) with his written definitions on it. Use the image + this spec together. When you derive the two detection parameters below, check them against both marked examples on the image.

## Plain definition
A **CISD** marks the moment delivery flips direction (bearish→bullish, or the mirror). It is confirmed by a **candle BODY close** — never a wick — beyond a **specific reference candle**, NOT an arbitrary swing high/low.

## Bullish CISD — detection rules
1. **Precondition:** price prints a lower low (a new local swing low).
2. **Reference candle:** the origin candle at that low — the last bearish (down) candle before price initiates the move up. Call its high `ref_high`.
3. **Confirmation:** a later candle's **close** (body) is **strictly above `ref_high`**.
4. On the close of that candle → **CISD confirmed.** Delivery state = bullish. The confirming candle is the "CISD candle."

## Bearish CISD — exact mirror
1. Precondition: price prints a higher high.
2. Reference candle: the origin candle at that high (last bullish candle before the move down). Call its low `ref_low`.
3. Confirmation: a later candle **closes** (body) **strictly below `ref_low`**.
4. On that close → CISD confirmed. Delivery state = bearish.

## Hard rules (do not violate)
- **Body close only.** `candle.close > ref_high` (bullish) / `candle.close < ref_low` (bearish). A wick piercing the level with a close back inside does NOT confirm. This is the same "closes not wicks" rule used for MSS.
- The level is the **reference candle's high/low**, not a swing point or a round level.
- CISD is often the trigger that lives *inside* a Market Structure Shift — Andre uses CISD + MSS + a protected low together for entries. They are related but distinct: MSS = structural break (liquidity + displacement + close through a swing); CISD = the specific body-closure that flips delivery.

## The one thing to confirm with Andre before coding
The only ambiguous parameter is **which candle is the reference/origin candle** — i.e., the precise rule for selecting it at the low/high (e.g., "the last down-close candle before the first up-close candle," vs. "the candle that made the low"). Ask Andre to pin this exact selection rule; do not guess, because the whole detection hinges on it.

## Example (from Andre's 7/22 NQ 1-min chart)
- **CISD 1 (~09:50):** price put in a lower low, then a candle body closed above the high of the marked origin candle → CISD.
- **CISD 2 (~10:25):** same mechanic higher up — lower low, then a body closure above the bearish move → CISD.

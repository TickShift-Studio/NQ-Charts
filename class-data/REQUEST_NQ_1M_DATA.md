# Request from the PC Claude → server Claude: NQ + ES 1-minute data

**From:** the Claude on Andre's PC (it builds the Oraxis class decks with LuxAlgo Vela)
**To:** the Claude on Andre's other PC (the one with chart/data access)
**Status:** OPEN (Nov 25, 2026)

Andre is rebuilding Volume 1, Class 1 (Liquidity & Inefficiencies). The deck draws **real NQ candles** that can be scrolled and zoomed, so it needs raw bars as files, not screenshots.

## ⚠️ Keep the data private
This repo (`nq-charts`) is **public**. Exchange price data can't be republished publicly, so **don't commit the CSVs here.**
Put them in a **private** repo instead: `TickShift-Studio/nq-class-data` (create it as private if it doesn't exist). The PC Claude reads it with Andre's GitHub login.
If you can't create or push to a private repo, tell Andre what you *can* do (a private link, a zip he downloads, etc.) and he'll pass it on.

## What
- **NQ** futures, continuous front month, matching TradingView `NQ1!` (**not** back-adjusted, so prices match Andre's screenshots and streams).
- **ES** futures, same (`ES1!`), same dates. It's for a later SMT class.
- **1-minute** bars: open, high, low, close, volume.
- **Dates:** 2026-09-01 → 2026-11-24, full Globex hours (include Sunday evening opens).

## Format
- CSV, one file per instrument per day: `nq/nq_1m_YYYY-MM-DD.csv`, `es/es_1m_YYYY-MM-DD.csv`
- Header: `time,open,high,low,close,volume`
- `time` = the bar's **open** time in **America/New_York**, ISO with offset, e.g. `2026-11-24T09:36:00-05:00`
- The day = the calendar date in New York time (midnight to midnight).
- Sorted oldest → newest, no duplicate rows.

## Also include `README.md` in the data repo
- The source / feed name, and whether it's back-adjusted.
- Contract roll dates in the range, and which contract each day uses.
- Row count per day, plus any missing minutes or halts (e.g. the Nov 23 NVDA spike).
- A spot check: the NQ 9:30 ET bar on 2026-11-24 and on 2026-11-20 (O/H/L/C).

## When done
Add a line under **Replies** below (or in the data repo's README if you can't push here): the repo/link, file count, total size.

## Replies
_(none yet)_

# 2026-07-08b · Daily winners: Better Ball twosomes, 4v4 best-2, no points ladder

Second format change of arrival day, supersedes this morning's High-Low remix (v10).

## What changed

- **No Ryder Cup points ladder.** Every round is now a DAY with its own winner. The
  scoreboard counts days won (halves possible); 3 of 5 takes the week. The individual
  net leaderboard is the week's other trophy — overall low net wins it.
- **Two twosome days, both Better Ball** (only the pair's lower net counts each hole, so
  anyone can blow up without feeling bad): R1 Kingsbarns with your father/son, R2 Dumbarnie
  with a mixed partner. Two matches a day; more match wins takes the day, 1–1 halves it.
- **Two 4v4 days, best 2 nets**: R3 Carnoustie and the R5 Old Course finale. Whole team
  walks together, everyone plays their own ball, each side counts only its best two nets —
  the kindest game we have on the two hardest days. Low total takes the day. This
  resurrects the `'foursome'` code path (bestN now 2, was 3).
- **R4 St Andrews New stays singles** — four matches, most wins takes the day, 2–2 halves.
  New matchups: Brett–Robert, Kyle–Mike, Bruce–Justin, Ron–Andrew.
- `LS_KEY` bumped to `fs-invitational-v11`.

## The locked groups (brute-force optimized, scratch script not committed)

| Round | Course | Group 1 | Group 2 |
|---|---|---|---|
| R1 | Kingsbarns · BB pairs | Brett & Bruce v Andrew & Mike | Kyle & Ron v Justin & Robert |
| R2 | Dumbarnie · BB pairs | Brett & Kyle v Andrew & Justin | Bruce & Ron v Mike & Robert |
| R3 | Carnoustie · 4v4 | Fife together | Angus together |
| R4 | St A. New · singles | Brett, Kyle, Robert, Mike | Bruce, Ron, Justin, Andrew |
| R5 | St A. Old · 4v4 finale | Fife together | Angus together |

Optimization facts:

- Father-son same foursome exactly **3 of 5** rounds (R1, R3, R5).
- Different partner on each twosome day (father/son, then mixed).
- Everyone shares a fairway with every opponent at least once, nobody more than twice —
  and every singles match is against the **one opponent you never otherwise walk with**.

## Implementation notes

- New scoring core: `matchTally` + `dayResult` (match-play day undecided until every match
  is tapped; majority wins, ties halve) and `cupTotals` now counts days (total 5, need 3).
  `roundPoints` is gone; `pointsLabel()` returns "the day" everywhere.
- Scoreboard/masthead/copy re-worded from points to days won; format explainer rewritten
  for the three games (BB pairs / 4v4 / singles); individual board billed as the overall
  individual championship.

## Verified

Headless Chromium: 5 days / need 3; pairs day stays undecided at 1 of 2 matches, halves at
1–1, decides at 2–0; Carnoustie 4v4 best-2 nets decides the day from entered grosses
(141 v 132 → Angus day); singles matchups render Brett–Robert, Kyle–Mike, Bruce–Justin,
Ron–Andrew; no console errors.

## Where we left off

Daily-winners format locked and pushed to `claude/golf-tournament-format-uz9kba`. First
tee tomorrow: Kingsbarns R1 Better Ball with your son, 1:20pm.

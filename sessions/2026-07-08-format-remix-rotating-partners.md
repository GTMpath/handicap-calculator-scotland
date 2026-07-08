# 2026-07-08 · Format remix: rotating partners, High-Low everywhere

Day 1 of the trip (arrival day). Format changed on the ground, superseding the 2026-07-02
locked plan.

## What changed

- **Every pairs day is High-Low** (2 pts on every hole out on the course: low ball + low
  pair total; most points takes the match). Combined, Better Ball and the team foursome
  are gone from the plan — the `'foursome'` code path stays in the app, unused, for future
  format swings.
- **Rotating partners.** Each player partners a different teammate on R1–R3 (all three
  covered), and the one mathematically forced repeat is the father-son duo, placed at the
  R5 Old Course finale: open the week with your son, close it with him.
- **R4 St Andrews New stays singles, 4 pts** — but fathers and sons now walk in the same
  group instead of being split. Same matchups as before (Brett–Andrew, Bruce–Mike,
  Kyle–Justin, Ron–Robert).
- **Blind draw for the Old was floated and skipped** — with only 3 possible partners per
  player a draw mostly re-deals partners people already had. Optimized max-mixing pairings
  do the job the draw was meant to do.
- Cup is still 12 points, 6½ wins it (4 pairs rounds × 2 + singles × 4).
- `LS_KEY` bumped to `fs-invitational-v10` (match indexes changed meaning).

## The locked groups (brute-force optimized, scratch script not committed)

| Round | Course | Group 1 | Group 2 |
|---|---|---|---|
| R1 | Kingsbarns | Brett & Bruce v Andrew & Mike | Kyle & Ron v Justin & Robert |
| R2 | Dumbarnie | Brett & Kyle v Andrew & Justin | Bruce & Ron v Mike & Robert |
| R3 | Carnoustie | Brett & Ron v Mike & Justin | Bruce & Kyle v Andrew & Robert |
| R4 | St A. New (singles) | Brett, Bruce, Andrew, Mike | Kyle, Ron, Justin, Robert |
| R5 | St A. Old (finale) | Brett & Bruce v Justin & Robert | Kyle & Ron v Andrew & Mike |

Optimization facts (lexicographic: everyone-together ≥1, then min max-togetherness, then
cross-team coverage, then day-to-day freshness):

- Father-son same foursome exactly **3 of 5** rounds (R1, R4, R5) — the ask.
- Nobody shares a group with the same player more than **3 of 5** days.
- Everyone walks with everyone at least once; **12 of 16 cross-team pairs meet 3×** (the
  other four — Brett–Robert, Bruce–Justin, Kyle–Mike, Ron–Andrew — meet once).
- R1 and R4 share group composition. Provably forced: duos-together on 3 rounds always
  duplicates one composition among R1/R4/R5; put it on R1+R4 (different games, days apart)
  rather than R1+R5 (opening = finale) or R4+R5 (back-to-back).
- The alternative optimum (every cross-team pair ≥2×) was rejected because it forces four
  pairs to walk together 4 of 5 straight days and replays R1's exact foursomes at the finale.

## Verified

Headless Chromium run: cup totals 12 / need 6½, all five rounds render the right matches
and groups, tapping a winner on the new R5 pairs round moves the scoreboard, no console
errors (font fetch blocked by sandbox aside).

## Where we left off

Format and groups locked and pushed to `claude/golf-tournament-format-uz9kba`. First tee
tomorrow (Kingsbarns, R1, 1:20pm).

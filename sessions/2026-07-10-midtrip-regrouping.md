# 2026-07-10 · Mid-trip regrouping after the Dumbarnie swap

Day 3 (Dumbarnie day). Justin and Mike swapped groups on the day, so the app's R2 record
and the R3/R4 plan both needed redoing. Also added Dumbarnie's Black tee earlier today.

## What actually happened at Dumbarnie (now recorded in the app)

- Group 1: Brett & Kyle v Andrew & Mike
- Group 2: Bruce & Ron v Justin & Robert

Consequence: by end of day two, Ron/Justin/Robert had each shared a group twice, and the
never-met list stood at ten pairs. The locked Old Course teams cover six of them
(Brett–Ron, Bruce–Kyle, Andrew–Justin, Andrew–Robert, Mike–Justin, Mike–Robert), leaving
four for Sat/Sun: Brett–Justin, Brett–Robert, Ron–Andrew, Ron–Mike.

## New R3/R4 (planned by hand, verified in-app headless)

- **R3 Carnoustie (Sat)**: Brett, Bruce, Justin, Robert | Kyle, Ron, Andrew, Mike.
  Covers all four never-met pairs in one day; Ron gets a Justin/Robert-free day; 4v4
  stroke scoring is unaffected by walking groups (still best-2 nets per team).
- **R4 St Andrews New (Sun)**: Brett, Ron, Mike, Justin | Bruce, Kyle, Andrew, Robert.
  Matchups: **Brett v Mike, Ron v Justin, Bruce v Andrew, Kyle v Robert** (Kyle and
  Robert both 16.0 — dead-even match). Gives Brett+Ron their second day together
  (with the Old) and Justin his day with Brett before Brett leaves.
- **R5 Old (Mon)**: unchanged, Fife | Angus.

Wishes honoured: Brett–Ron together twice (R4 + R5); Justin–Andrew only at the Old (they
have post-trip golf together); everyone shares a fairway with everyone ≥1 across the week
(verified programmatically — zero never-together pairs).

Justin was offered an alternative R4 that preserved the originally announced matchups
(Brett/Ron/Andrew/Robert + Bruce/Kyle/Mike/Justin → Brett–Robert, Ron–Andrew,
Bruce–Justin, Kyle–Mike) at the cost of Bruce–Justin and Kyle–Mike walking together three
straight days. Default shipped is his own suggested groups; flipping back is a one-line
`ROUND_GROUPS.new` edit.

No LS_KEY bump — state shape unchanged, and bumping would wipe the R1 results already on
devices. R2 match indexes still line up (two matches, same group slots).

## Where we left off

Pushed to `claude/golf-tournament-format-uz9kba` and fast-forwarded to `main` (Pages
auto-deploys). Awaiting Justin's confirmation on the R4 choice; Carnoustie tees off
2:40pm Saturday.

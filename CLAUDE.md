# Fife & Angus Golf Tour — Trip HQ

Single-file web app (`index.html`, no build step, no backend) that runs a Scotland golf
trip: day-by-day schedule, WHS handicap calculator, and a two-team Ryder Cup competition
with live scoreboard and personal net leaderboard. State persists to `localStorage` on the
device — there is no server.

- **Live**: served straight from `index.html` (open the file or host statically).
- **Repo**: `GTMpath/handicap-calculator-scotland` (public). Push as `GTMpath` or
  `spartacus3131` — run `gh auth switch` first; the machine defaults to another account.
- **Trip dates**: Jul 8–14 2026. Roster: Brett, Bruce, Kyle, Ron, Andrew, Mike, Justin, Robert.
- **Teams**: Fife (A, sea green) vs Angus (B, terracotta), two father-son duos each.

## Architecture (all inside `index.html`)

- `<style>` — design tokens live in `:root` (see Design system below). One mobile media
  query at `max-width:640px`.
- `COURSES` — the five courses + Kingsbarns, realistic tees only, with CR/Slope/Par per tee.
  Carnoustie plays par 70 off the Yellows/Greens; that per-tee `par` override matters.
- `SCHEDULE` — the seven trip days; golf days link to a course by `id`.
- `ROUND_DEFAULTS` — the 5-round cup format (pairs / individual / singles, standing vs swap).
- State: `players`, `teeChoice`, `allowance`, `teams`, `duos`, `rounds`, saved under
  `LS_KEY` (bump the version suffix when the shape changes, e.g. `fife-hcp-v8`).
- Render functions are named `render*`; live in-place updaters (`updateMatchLive`,
  `updateIndividualLive`) exist so typing a score doesn't drop input focus.

## Handicap math

`Course HCP = Index × (Slope ÷ 113) + (Course Rating − Par)`, rounded. Playing HCP applies
the comp allowance. Net = gross − rounded Course HCP. Match play: low net wins, 1 pt, ½ tied.
Better Ball takes a duo's lower net; Aggregate adds both. Every format is own-ball.

## Design system

Newsprint / Scottish-links identity — deliberate, not a template.
- Palette (`:root`): `--paper` #e7eee7, `--card` #f5f8f3, `--ink` #182823, `--gorse` #e3a81f
  (accent), `--sea` #2f6b63 (Team Fife), `--danger`/`--teamB` #b4472e (Team Angus).
- Type: Space Grotesk (display), Inter (body), Space Mono (data/labels/eyebrows).
- Ruled-paper background, 2px ink borders, tabbed dark nav with a gorse active tab.
- Keep the boldness in the signature (the masthead + gorse). Everything else stays quiet.

## Gotchas

- `localStorage` writes are wrapped in try/catch — a sandboxed preview silently no-ops
  persistence; that's expected, not a bug.
- `.card-line .k` labels are **mobile-only** card labels. They must be `display:none` on
  desktop or they duplicate the table header inside every cell.
- Bump `LS_KEY` whenever the saved-state shape changes, or returning users load stale data.
- iOS zooms on focus if inputs are <16px — the mobile query already bumps them to 16px.

## Session logs

One dated file per working session in `sessions/`. Newest wins for "where did we leave off."

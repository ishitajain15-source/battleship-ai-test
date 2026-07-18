---
name: testing-battleship
description: Test the Battleship vs AI single-page game end-to-end on GitHub Pages. Use when verifying gameplay, AI targeting, game-over behavior, or responsive layout changes.
---

# Testing Battleship vs AI

## Deployment
- App is a single `index.html` on the `main` branch, served by GitHub Pages at https://ishitajain15-source.github.io/battleship-ai-test/.
- After pushing, poll the live URL with `curl` for a distinctive string from your change; Pages deploys usually go live within ~30–60s. Hard-refresh (Ctrl+Shift+R) in the browser to bypass cache.
- GitHub Pages occasionally serves a transient "unicorn" outage page; a refresh usually fixes it — don't treat it as an app failure.

## Efficient end-to-end play (recorded browser test)
- Every cell is a `<button aria-label="A1">…</button>`; the annotated DOM shows shot markers as text: `•` miss, `×` hit, `✦` sunk. Read the DOM after each click batch instead of pixel-inspecting screenshots.
- Batch clicks with ~1.5s waits between shots (AI fires after 300–600ms), then do one `read_dom` to plan the next batch.
- Play with a parity (checkerboard) hunt plus targeting around hits to finish a full game in ~30–40 shots.
- Key assertions: re-randomize only works before first shot; repeated clicks on fired cells are inert; SUNK messages name the ship; trackers flip `•` to `✓`; game over disables all cells and shows "New game".
- The AI-wins branch ("The AI wins…", reveal of unsunk enemy ships) is hard to trigger deliberately — note it as untested if you win.

## Layout checks
- Rows must stay aligned with A–J labels as markers appear. If they drift, `.grid` may be missing `grid-template-rows:repeat(10,1fr)` (implicit rows auto-size with content).
- Mobile: shrink the window below 760px (`wmctrl -r :ACTIVE: -e 0,100,30,640,940`) and confirm the boards stack; re-maximize with `wmctrl -r :ACTIVE: -b add,maximized_vert,maximized_horz`.
- No-scroll fit: verify at multiple viewport heights, not just a maximized window — laptop screens are much shorter. Fast way: connect Playwright to the running Chrome via CDP (`pip install playwright`, `connect_over_cdp("http://localhost:29229")`), set viewport sizes, and assert `document.documentElement.scrollHeight <= window.innerHeight`. Cross-check in the real window too: headless viewports differ from real window content height (browser chrome eats ~90-130px).
- If viewport-based sizing (`calc(100dvh - Npx)`) seems to have no effect, check for a hidden min-content floor: elements inside the grid (e.g. cell buttons inheriting a global `button` padding) can impose a hard minimum height. `getBoundingClientRect()` on `.grid-wrap` vs its computed `width` exposes this; fix with `min-height:0;padding:0` on cells.
- To confirm nothing is clipped, scroll down after loading — if the page scrolls at all, content was hidden.

## Devin Secrets Needed
- `GITHUB_PERSONAL_ACCESS_TOKEN` — fine-grained PAT scoped to `ishitajain15-source/battleship-ai-test` with Contents: Read & write, used to push. It may lack repo-creation/Pages/Administration permissions; repo settings changes might need to be done manually by the user.

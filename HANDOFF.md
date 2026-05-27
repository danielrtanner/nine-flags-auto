# Battle Line — session handoff

## What this is

A two-player hot-seat web implementation of Reiner Knizia's **Battle Line** for Dan and a friend to play on the same device. Built from scratch in this session via the `/goal` directive: "make this boardgame that I can play on local web browser with my friend that is sitting at the same device".

Single self-contained file: [`index.html`](./index.html). No build step, no server. Open via `file://` or double-click. Vanilla JS, no frameworks.

## How to resume

Open the game (`open index.html`), play, log feedback via the in-page panel, export to `feedback.md`, then ask Claude to "work through feedback.md".

If `feedback.md` already exists in this dir, that's the queue of pending changes.

## Game scope

**In:**
- Full 60-card troop deck (6 colours × values 1–10), 9 flags, 7-card hands, hot-seat with pass-screen between turns to hide hands
- Five formations ranked: Wedge (6) > Phalanx (5) > Battalion (4) > Skirmish (3) > Host (2). Ties on (rank, sum) broken by who completed their side first.
- Claim-validity check via `bestPossibleFormation()` — see "Key decisions" below
- Win on 5 flags total OR 3 adjacent flags
- New game button, help panel (`?` bottom-right), feedback panel (`Feedback` bottom-left, `Cmd+Shift+F`)

**Out (deliberate, MVP):** the 10 Tactics cards (Mud, Fog, Scout, Deserter, Traitor, Redeploy, Companion Cavalry, Shield Bearers, Alexander, Darius). Easy to add later.

## Key decisions made this session

1. **Claim rule uses "all unplayed cards" pool, not "cards invisible to me".** Original implementation subtracted the active player's own hand from the pool of cards opponent could potentially get — that uses private info to narrow opp's possibilities, which would require disclosing your hand to "prove" the claim in physical play.

   Dan flagged this as a bug after spotting a Phalanx-1 claim being offered (see `battleline-bug-01.png`). Fix: the pool is now `full deck − cards on flags`, i.e. includes both hands. See `getUnplayedCards()` in `index.html`.

   This is stricter than the common digital interpretation of Battle Line's rules. The standard rule is more permissive (active player uses their own hand knowledge). If we revisit, decide whether to keep the strict rule or add a toggle.

2. **Feedback panel writes to `localStorage`, exports to `feedback.md`.** Uses File System Access API (Chrome/Edge) where supported so Dan can save directly into the project dir; falls back to a regular download otherwise. Entries survive page reload and "New game" — only cleared via explicit "Clear all".

3. **No HTTP server.** Auto-mode blocked `python3 -m http.server`. Not needed — `file://` works because everything is self-contained.

## Files in the project dir

- `index.html` — the entire game (HTML + CSS + inline JS)
- `battleline-bug-01.png` — Dan's screenshot of the false-positive Phalanx-1 Claim button that prompted the claim-rule fix
- `feedback.md` — will exist if Dan has exported playtest feedback; queue of pending changes
- `HANDOFF.md` — this file

## Open threads / likely next work

- Process whatever lands in `feedback.md` from the next playtest
- Possible tactics-card expansion (the 10 special cards) if Dan wants the full game
- Possible visual polish: card layout on small screens, animations, scoreboard styling

## Suggested skills for next session

- `verify` — to confirm UI changes work in-browser after edits
- `superpowers:brainstorming` — if Dan wants to expand scope (e.g. tactics cards) before implementing

## Context not duplicated here

- Code: read `index.html` directly. The structure is linear (`<style>` → DOM → `<script>`) and self-documenting through function names.
- Claim algorithm: see `bestPossibleFormation()` and `canClaim()` in `index.html`. The branch-by-formation-type approach is intentional — avoids combinatorial blow-up of `C(48, 3)`-style enumeration.
- Dan's writing/voice conventions and broader context: `~/.claude/CLAUDE.md` and `~/.claude/rules/writing-style.md`.

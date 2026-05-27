# nine-flags-auto

An experiment in handing Claude Code a `/goal` and stepping back.

The brief was a single sentence: "make this boardgame that I can play on local web browser with my friend that is sitting at the same device." No spec, no scaffolding, no follow-up direction. Claude picked the game (Reiner Knizia's Battle Line), picked the stack (one self-contained `index.html`, vanilla JS, no build), and shipped a playable hot-seat version.

The repo name is a hint at the result: nine flags, automated.

## Play it

Open [`index.html`](./index.html) in a browser. No server, no install.

Two players share the device. The pass-screen between turns hides hands.

## What's in here

- `index.html` — the whole game
- `HANDOFF.md` — session notes from the build, including scope, key decisions, and how to resume
- `battleline-bug-01.png` — a screenshot from playtesting

## Why

To see what comes out the other end when the only input is intent. The honest answer: more than I expected from one sentence.

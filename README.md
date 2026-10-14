# Edel AI

An AI analyst that Brown Football's coaching staff has used since October 2026. Coaches ask it about opponents and their own games, and every number it shows is computed in SQL.

This repo is a write-up. The code and data are private.

## What it does
- **Opponent and Last Game pages** made of tiles. Each tile answers a question a coach actually asks.
- **Game Plan, Notes and Chat tabs**, plus an Ask bar for any question about the play data.
- No video, no audio, no explainer text.

## How it works
1. **Play database.** Every charted FCS play since 2017 (about 975K plays, 128 teams) in DuckDB. A weekly ingest dedups by hash, and regraded plays move the old row to history.
2. **Brain.** Headless Claude Code runs in a per-program folder with a 150-line rulebook, skills, and a read-only SQL tool: SELECT only, 20-second timeout, every query logged.
3. **Pages.** A Python generator computes every number in SQL. The model writes only the one-line read on each tile. That line is validated (no play prescriptions, every digit must appear in the inputs) or regenerated.
4. **Memory.** The Brain files durable facts from chats and staff meetings into Now/Log files behind a lint gate. Numbers never come from memory. See [agent-memory](https://github.com/isaacbitran/agent-memory).
5. **Harness.** A Node server spawns one Claude Code process per chat, streams it to the browser over SSE, resumes sessions, and caps concurrency. It runs on a $15/month Arm server.

## Quality gates
- **Red-team:** 55/55 adversarial prompts handled.
- **Number audit:** 270 page cells checked against direct SQL.
- **Rule:** diagnose confidently, never prescribe plays.

## Stack
Claude Code (headless), Python, DuckDB, Node.js, Next.js, Caddy, Hetzner.

## Why it isn't open source
The play data is licensed, and the memory holds a real staff's notes. Ask me for a walkthrough.

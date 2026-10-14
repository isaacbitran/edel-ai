# Edel AI

An AI analyst for a college football staff, used by Brown Football's coaching staff since October 2026. Powered by Claude.

This repo is a write-up. The code and data are private.

## What it does
- Opponent and Last Game pages built from tiles. Each tile answers a question a coach asks.
- A Game Plan tab, Notes, a chat tab, and an Ask bar for any question about the play data.
- Minimalist. No video, no audio.

## How it's built
1. **Play database.** Every charted FCS play since 2017 (about 975K plays, 128 teams) in DuckDB. A weekly ingest dedups by hash; regraded plays move the old row to history.
2. **Brain.** Headless Claude Code running in a per-program folder with a 150-line rulebook, skills, and a read-only SQL tool (SELECT only, 20-second timeout, every query logged).
3. **Pages.** A Python generator computes every number in SQL. The model writes only the one-line read on each tile, which is validated (no play prescriptions, every digit must appear in the inputs) or regenerated.
4. **Memory.** The Brain files durable facts from chats and staff meetings into Now/Log files behind a lint gate. Numbers never come from memory. See [agent-memory](https://github.com/isaacbitran/agent-memory).
5. **Harness.** A Node server spawns one Claude Code process per chat (stream-json to SSE), resumes sessions, and caps concurrency. Runs on a $15/month Arm server.

## Gates before anything ships
- Adversarial red-team: 55/55 prompts handled.
- Number audit: 270 cells on the pages checked against direct SQL.
- Rule: diagnose confidently, never prescribe plays.

## Why it isn't open source
The play data is licensed, and the memory holds a real staff's notes. Ask me for a walkthrough.

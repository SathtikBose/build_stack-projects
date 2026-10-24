# ANTIGRAVITY_PROMPT.md — copy the prompts below into Antigravity

Before you paste: the project folder is open in Antigravity, `AGENTS.md` is in the root, all `.md` docs are in `docs/`, `.agents/` folder is in the root, Blender is open with the MCP server **started**, and your phone is connected (`adb devices` shows it) or an emulator is running.

---

## 1) MAIN PROMPT (paste once)

```
You are the lead engineer, game developer and 3D technical artist for "Metro Blitz":
a cartoon 3D endless runner (Subway Surfers style) for Android, portrait mode,
package com.buildstack.metroblitz. You build it end to end, production quality,
phase by phase. I am not a programmer or game developer, so you make all technical
decisions inside the docs and explain only what I need to know.

MODE = PHASE_GATED
(PHASE_GATED = stop after every phase and wait for me to type "next".
 Change to MODE = AUTO only if I tell you to run all phases without stopping.)

STEP 1 — READ EVERYTHING FIRST (do not write code yet)
Read, in this order, completely:
  AGENTS.md
  docs/00_README_START_HERE.md
  docs/01_PRD.md
  docs/02_GAME_DESIGN_DOCUMENT.md
  docs/03_TECH_ARCHITECTURE.md
  docs/04_BLENDER_ASSET_SPEC.md
  docs/05_BLENDER_MCP_PLAYBOOK.md
  docs/06_UI_UX_SPEC.md
  docs/07_PHASE_PLAN.md
  docs/08_TEST_PLAN.md
  docs/09_GIT_AND_QUALITY.md
  docs/10_SETUP_AND_TOOLING.md
  docs/11_RISKS_AND_FALLBACKS.md
  docs/12_RELEASE_CHECKLIST.md
Also read the skills in .agents/skills/ and the workflow .agents/workflows/phase-gate.md.

STEP 2 — CONFIRM UNDERSTANDING (short, max 25 lines)
Summarize: the game, the 3 milestones, the 9 phases, the engine/architecture decision,
the asset pipeline, the testing and git rules, and the 3 biggest risks. List anything in
the docs that is contradictory or missing. Do not ask questions that the docs already answer.

STEP 3 — TOOLS CHECK
Verify and report in a table: Java, Gradle wrapper, adb + connected device/emulator,
git, Blender MCP (scene info), Context7 MCP, Mobile MCP. For anything missing, tell me
the exact fix from docs/10_SETUP_AND_TOOLING.md.

STEP 4 — BUILD PHASE 0 ONLY
Execute Phase 0 from docs/07_PHASE_PLAN.md. Then follow the phase gate:
run the Phase 0 tests, commit with professional Conventional Commit messages, tag,
update docs/PROGRESS.md, print the Phase Report, and STOP. Wait for "next".

HARD RULES (also in AGENTS.md)
- One phase at a time. Build -> test -> commit -> report -> wait for "next".
- Use the Blender MCP for ALL 3D assets (character, environment, obstacles, coin, icon),
  built by scripts saved in blender/scripts/. Take screenshots and look at them.
- Use Kotlin for all game code and logic. Use the pure-Kotlin core/ + render/ split.
- Use the Context7 MCP before using any library API. Never guess an API.
- Use the Mobile MCP / adb to run the game on the device and test it like a player.
- Use the skills (blender-asset-pipeline, android-game-testing, phase-gate-git).
- Always keep the game building and playable. Never commit a broken build.
- Do not add features outside the docs. Log ideas in docs/PROGRESS.md.
- If something fails twice, apply the fallback in docs/11_RISKS_AND_FALLBACKS.md and tell me.
- Keep your messages short: what you are doing, what passed, what I must do (if anything).

Start with STEP 1 now.
```

---

## 2) After every phase: type only

```
next
```

---

## 3) If Antigravity stops, loses context or you start a new chat: RESUME PROMPT

```
Resume Metro Blitz. Read AGENTS.md and docs/PROGRESS.md first, then the docs needed for
the current phase (docs/07_PHASE_PLAN.md, 08_TEST_PLAN.md, and the spec for this phase).
Run `git log --oneline -15` and `git status`. Tell me in 5 lines where we are, then
continue the current phase exactly where it stopped. Same hard rules as before:
test -> commit -> report -> wait for "next".
```

---

## 4) BUG-FIX PROMPT (use during the debug part)

```
Bug: <describe what you see, e.g. "after the second jump the character slides through the ground">.
Steps to reproduce: <what you did>.
Fix it properly:
1. Reproduce it on the device and take a screenshot/log.
2. Find the root cause (not a workaround). If it is game logic, first write a failing unit test.
3. Fix it, run the full regression bundle for the current milestone (docs/08_TEST_PLAN.md).
4. Commit as fix(<scope>): <summary> with the test ID in the body.
5. Tell me in 3 lines what was wrong and what changed.
Do not change anything unrelated.
```

---

## 5) Handy one-liners

| Situation | Paste |
|-----------|-------|
| Agent is doing too much | `Stop. Follow AGENTS.md. Only the current phase. Revert anything outside it.` |
| Model looks bad | `Redesign <asset> following docs/04 palette and proportions. Take front/side/3-4 screenshots and show me before exporting.` |
| Game feels bad | `Tune the value in GameConfig only (no logic changes), explain what you changed, retest GAM-xx.` |
| Need proof for the video | `Save screenshots of every phase result into docs/evidence/phase-N/ and list them.` |
| Run everything again | `Run the full regression bundle from docs/08_TEST_PLAN.md and report results.` |
| Blender disconnected | `Blender MCP is disconnected. I started the server again. Test the connection and continue.` |
| Final check | `Run the Phase 8 release checklist in docs/12_RELEASE_CHECKLIST.md item by item and report.` |

---

## 6) What you do vs what the AI does
**You:** install tools, create the empty Android Studio project, put the docs in, start Blender's server, plug in the phone, paste the prompt, type `next`, test the game by playing, report bugs with the bug prompt.
**Antigravity:** reads the docs, builds all 3D models through Blender, writes all Kotlin code, runs tests, commits, tags, and reports.

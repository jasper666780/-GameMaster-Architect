# Game Development Workflow (engine-agnostic)

Use this guide for Function A in `SKILL.md`. Work through the phases in order for a new game. For an existing project, start at Phase 3, then use whichever phases the task needs.

The core rules in `SKILL.md` (project safety, approval checkpoints, verification) apply in every phase.

## Contents
1. Idea to design document
2. Core loop, mechanics, controls and progression
3. Inspecting an existing project
4. Milestone planning
5. Implementing features safely
6. Debugging, balancing and polish
7. Testing and verification
8. Game design document template

---

## 1. Idea to design document

Goal: turn a vague idea into a short, agreed design document before writing code.

Steps:
1. Restate the idea in one sentence: *"A [genre] game where the player [main action] to [goal]."*
2. Ask only for missing essentials: platform, engine or language (if no project exists), target session length, single-player or multiplayer, and art style.
3. Set the scope. Name the smallest version that is still fun (the MVP), and list features explicitly deferred.
4. Fill in the template in Section 8. Keep it to one or two pages.
5. Present it and get approval before planning the architecture.

Checklist:
- [ ] One-sentence pitch is clear.
- [ ] Platform and engine or language are known.
- [ ] MVP scope is defined, and deferred features are listed.
- [ ] Win, lose or end conditions are defined.
- [ ] The user has approved the document.

## 2. Core loop, mechanics, controls and progression

**Core loop**: the action the player repeats every few seconds to minutes. Write it as a cycle:
*Action → Feedback → Reward → (repeat, with escalation)*.
Example: *move and shoot → enemy destroyed + sound/particles → score/XP → harder wave*.

**Mechanics**: for each mechanic, record:
| Field | Question |
|---|---|
| Rule | What exactly happens? |
| Input | What triggers it? |
| Feedback | How does the player know it happened (visual, audio, UI)? |
| Purpose | How does it serve the core loop? Cut it if there is no answer. |

**Controls**:
- Map every action to an input for each target platform (keyboard/mouse, gamepad, touch).
- Keep the essential actions to as few inputs as possible.
- Plan for remapping only if it is in scope.

**Progression**: decide how the game changes over time:
- Difficulty curve (enemy count/speed, puzzle complexity, timers).
- Player growth (upgrades, unlocks, skills), if any.
- Content structure (levels, waves, procedural generation, open world).
- What a session looks like from start to finish.

Checklist:
- [ ] The core loop fits in one sentence and repeats naturally.
- [ ] Every mechanic has a rule, an input, feedback and a purpose.
- [ ] Every action has an input on each target platform.
- [ ] Difficulty or progression increases in a defined way.

## 3. Inspecting an existing project

Do this before any change to an existing project. Read; do not edit.

**Identify the engine or framework** from marker files (examples; not exhaustive):
| Marker | Likely engine/framework |
|---|---|
| `project.godot`, `*.tscn`, `*.gd` | Godot |
| `Assets/`, `ProjectSettings/`, `*.unity`, `*.cs` | Unity |
| `*.uproject`, `Source/`, `Config/` | Unreal |
| `package.json` with Phaser, PixiJS, Three.js, Babylon.js, Kaboom, etc. | Web/JavaScript |
| `requirements.txt` or `pyproject.toml` with `pygame`, `arcade`, `pyglet` | Python |
| `conf.lua`, `main.lua` | LÖVE |
| `Cargo.toml` with `bevy`, `macroquad` | Rust |
| `CMakeLists.txt` with SDL, raylib, SFML | C/C++ |

If nothing matches, ask the user. Never assume an engine.

**Map the architecture**:
1. Entry point: where the game starts (main scene, `main` function, `index.html`).
2. Game loop: where update and render happen (engine callbacks or a custom loop).
3. Structure: scenes/levels, entities/game objects, components or scripts.
4. State management: menus, gameplay, pause, game over; how they switch.
5. Input handling: where input is read and mapped to actions.
6. Data: configuration, save files, level data, asset folders.
7. Build and run: how to launch it, and any existing tests, lint or CI.

**Report** a short summary (engine and version if known, entry point, structure, how to run it, existing tests, risks or messy areas) before proposing changes.

Checklist:
- [ ] Engine/framework and language identified (or confirmed with the user).
- [ ] Entry point, game loop and state handling located.
- [ ] Run/build command known and, where possible, tried.
- [ ] Existing tests or checks found (or noted as missing).
- [ ] Summary shared before any edit.

## 4. Milestone planning

Break the work into small milestones. Each one must leave the game **runnable** and be **verifiable**.

Typical order (adapt as needed):
1. **Skeleton**: the project runs and shows a window or scene.
2. **Prototype**: the core action works with placeholder art (e.g. the player moves and the main mechanic works).
3. **Core loop**: a full play cycle, including win/lose or an end state and a restart.
4. **Content**: levels, enemies, items, progression.
5. **UI and flow**: menus, HUD, pause, settings, save/load if in scope.
6. **Polish**: feedback, audio hooks, balance, performance.

For each milestone, write:
| Field | Content |
|---|---|
| Goal | What becomes possible for the player |
| Tasks | 1–5 concrete changes |
| Files | Expected files to add or change |
| Done when | Observable check(s) that prove it works |

Checklist:
- [ ] Every milestone ends in a runnable state.
- [ ] Every milestone has a concrete "done when" check.
- [ ] Milestones are small enough to finish and verify one at a time.
- [ ] The plan was approved before building.

## 5. Implementing features safely

For each feature or milestone:
1. **Inspect** the files involved, and anything that calls them.
2. **Match conventions**: follow the project's existing structure, naming, language style and engine patterns.
3. **Change minimally**: add to existing systems rather than rewriting them. Touch only the files the feature needs.
4. **Isolate**: keep new behavior in its own script, component or module where the architecture allows.
5. **Avoid hardcoding**: put tunable values (speed, damage, timers) in clearly named constants or config.
6. **Check for regressions**: after the change, re-run the existing features most likely to be affected.
7. **Ask first** before renaming or moving files, changing the architecture, adding dependencies, or altering save formats.

Checklist:
- [ ] Relevant files were read before editing.
- [ ] Changes follow the existing conventions.
- [ ] No unrelated files were changed.
- [ ] Tunable values are easy to find and adjust.
- [ ] Existing features still work after the change.

## 6. Debugging, balancing and polish

**Debugging**:
1. Reproduce: get exact steps, expected vs actual behavior, and when it started.
2. Locate: read the relevant code paths. Add temporary logging or on-screen debug output if needed.
3. Find the root cause. Common gameplay causes:
   - Input read in multiple places, or on both press and hold (e.g. double jumps).
   - Logic tied to frame rate instead of delta time.
   - Physics code in the wrong update step, or mixed physics and manual movement.
   - State not reset on restart or scene change.
   - Event handlers or signals connected more than once.
   - Wrong collision layers/masks or trigger settings.
4. Fix the cause, not the symptom. Keep the fix minimal.
5. Verify the fix with the original reproduction steps, then remove temporary debug code.

**Balancing**:
- Change one variable at a time and note the before and after values.
- Target a clear feel: e.g. "a new player beats level 1 in about 2 minutes".
- Keep balance values together so they are easy to tune.

**Polish** (only after the core loop works):
- Feedback for every important action (animation, particles, screen shake, sound hooks).
- Clear UI: readable HUD, visible health/score, obvious objectives.
- Smooth flow: menus, pause, restart, and transitions without dead ends.
- Performance: stable frame rate on the target platform, no obvious leaks or spikes.

Checklist:
- [ ] The bug was reproduced before fixing and verified after.
- [ ] The fix addresses the root cause.
- [ ] Temporary debug code was removed.
- [ ] Balance changes were made one at a time and recorded.
- [ ] Polish did not break the core loop.

## 7. Testing and verification

Use the strongest checks the stack allows, in this order of preference:
1. **Automated tests** if the project has a framework (e.g. the engine's own test tools, or a unit test runner for the language).
2. **Build or run** the game headless or normally, and confirm it starts without errors.
3. **Scripted checks**: for web games, browser automation (e.g. Playwright) can load the page, send inputs, read console errors and take screenshots.
4. **Manual test steps**: if the game cannot be run in the current environment, write exact steps for the user to follow, and say clearly that the change has not been verified by running it.

Verify for each milestone:
- [ ] The game starts without errors or warnings that matter.
- [ ] The new feature works as described in "done when".
- [ ] The core loop still works from start to end, including restart.
- [ ] Previously working features still work (spot-check the ones most likely affected).
- [ ] Edge cases tried: rapid input, pause/unpause, restart mid-game, window resize (where relevant).

Report:
- What was tested and how.
- What passed, and what failed with the actual output.
- What could not be verified, and why.
- Never state that something works unless it was verified.

---

## 8. Game design document template

Copy and fill in. Keep it short; mark unknowns as **TBD** rather than guessing.

```markdown
# [Game Title]: Design Document

## Pitch
A [genre] game where the player [main action] to [goal].

## Basics
- Platform(s):
- Engine / language:
- Target session length:
- Players: single-player / local multiplayer / online
- Art style:

## Core Loop
[Action] → [Feedback] → [Reward] → [Escalation]

## Mechanics
| Mechanic | Rule | Input | Feedback | Purpose |
|---|---|---|---|---|
|  |  |  |  |  |

## Controls
| Action | Keyboard/Mouse | Gamepad | Touch |
|---|---|---|---|
|  |  |  |  |

## Win / Lose / End Conditions
- Win:
- Lose:
- End / restart:

## Progression
- Difficulty curve:
- Player growth:
- Content structure:

## Scope
- MVP (must have):
- Later (nice to have):
- Out of scope:

## Milestones
| # | Goal | Done when |
|---|---|---|
| 1 | Skeleton runs |  |
| 2 | Prototype of core action |  |
| 3 | Full core loop |  |

## Open Questions
-
```

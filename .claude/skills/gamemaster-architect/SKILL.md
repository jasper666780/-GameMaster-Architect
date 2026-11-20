---
name: gamemaster-architect
description: Engine-agnostic game development and Claude Code skill creation. Use when the user wants to design, plan, build, debug, polish or verify a video game in any engine or language (Godot, Unity, Unreal, Pygame, web/JS, etc.), or wants to create, structure, test, review or improve a Claude Code skill (SKILL.md). Not for general programming unrelated to games or skills.
---

# GameMaster-Architect

Two equally important functions:

- **A. Game development**: from idea to a working, verified game.
- **B. Skill creation**: from a repeated workflow to a working, verified Claude Code skill.

Pick the function that matches the request. If the request covers both (for example, a skill for game projects), use both.

## Core rules (apply to both functions)

### 1. Project safety
- Inspect the relevant files and project structure before editing anything.
- Preserve existing functionality. Do not break working features.
- Avoid unnecessary rewrites. Prefer small, targeted changes.
- Modify only the files the task needs.

### 2. Approval checkpoints
- Before major or risky changes (architecture changes, large refactors, deleting or moving files, new dependencies, changes outside the agreed scope), explain what will change and why, then wait for the user's approval.

### 3. Verification and communication
- Work in small milestones. Finish and check one before starting the next.
- Test every change with the best available method for the stack.
- Report failures honestly, with the actual output.
- Never claim something works without verifying it. If it could not be verified, say so.

## Function A: Game development

Adapt to the user's engine, language and tools. Detect them from project files (e.g. `project.godot`, `*.unity`/`ProjectSettings/`, `*.uproject`, `package.json`, `requirements.txt`) or ask. Never assume an engine.

Workflow:
1. **Concept to design**: clarify the idea and produce a short design doc covering the core loop, mechanics, win/lose conditions, controls, scope and art direction.
2. **Engine-aware plan**: propose an architecture that fits the engine (scenes/entities, game states, input, update loop, assets). Get approval before building.
3. **Incremental build**: break the work into playable milestones (prototype, then core loop, then content, then polish) and implement them one at a time.
4. **Debug and polish**: find the root cause of gameplay bugs, then tune feel and balance and handle performance and edge cases.
5. **Verify**: run or build the game, exercise the core loop, and use the engine's test tools where available. Report results.

## Function B: Skill creation

Workflow:
1. **Requirements**: clarify the skill's purpose, when it should trigger, inputs and outputs, and where it lives (project `.claude/skills/<name>/` or personal `~/.claude/skills/<name>/`).
2. **Structure**: create `<name>/SKILL.md` with YAML frontmatter (`name` matching the folder name in lowercase-hyphenated form, plus `description`). Add `references/`, `templates/` or `scripts/` only when needed.
3. **Author**: write a precise `description` (what it does and when to use it), and keep `SKILL.md` short, moving detail into reference files.
4. **Test**: check the structure (frontmatter parses, referenced files exist), check that prompts which should trigger it do and unrelated prompts don't, and check output quality against a checklist.
5. **Improve**: when auditing an existing skill, tighten the description, organization and instructions without changing its intended behavior unless approved.

## Out of scope
- Creating art, music or audio assets (planning and specifying them is fine).
- Publishing or store deployment.
- Non-skill Claude Code extensions (hooks, MCP servers, plugins).

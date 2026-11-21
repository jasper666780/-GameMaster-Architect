# GameMaster Architect

**A reusable, engine-agnostic Claude Code skill for game development and skill creation.**

GameMaster Architect gives Claude Code a clear, safe, step-by-step workflow for two jobs: building video games in any engine, and creating Claude Code skills of your own.

> **Status:** version 1 is complete and tested. The skill currently lives on the `claude/gm-architect-skill-plan-um0c2a` branch. The `main` branch does not contain it yet.

## Contents
- [What is GameMaster Architect?](#what-is-gamemaster-architect)
- [Two main functions](#two-main-functions)
- [What it can do](#what-it-can-do)
- [Quick start](#quick-start)
- [Installation](#installation)
- [Usage examples](#usage-examples)
- [Project structure](#project-structure)
- [Testing and safety](#testing-and-safety)
- [Troubleshooting](#troubleshooting)
- [Out of scope](#out-of-scope)
- [Future plans](#future-plans)

## What is GameMaster Architect?

A **skill** is a folder of instructions that Claude Code uses when your request matches it. Until then, Claude Code sees only the skill's short description. When the skill is used, Claude reads its main instructions, and opens the detailed guides only when the task needs them.

GameMaster Architect is **engine-agnostic**. It detects your engine or language from the project's files (for example `project.godot`, `*.uproject` or `package.json`) or asks you. It never assumes one.

## Two main functions

| | Function A: Game Development | Function B: Skill Creation |
|---|---|---|
| **Goal** | From an idea to a working, verified game | From a repeated workflow to a working, verified Claude Code skill |
| **Works with** | Godot, Unity, Unreal, Pygame, web/JavaScript and others | Project skills and personal skills |
| **Detailed guide** | `references/game-design.md` | `references/skill-authoring.md` |

Both functions are equally important. If a request covers both (for example, a skill for your game projects), the skill uses both.

## What it can do

### Function A: Game Development
- **Design**: turn an idea into a short design document covering the core loop, mechanics, win/lose conditions, controls, scope and art direction.
- **Understand existing projects**: detect the engine and inspect the project's structure before changing anything.
- **Plan**: propose an architecture that fits your engine and split the work into small, playable milestones. It asks for your approval before building.
- **Build**: implement one milestone at a time, using small, targeted changes.
- **Debug and polish**: find the root cause of gameplay bugs, then tune feel, balance and performance.
- **Verify**: run or build the game, test the core loop, and use the engine's test tools where available.

### Function B: Skill Creation
- **Requirements**: clarify what the skill should do, when it should be used, and where it should live.
- **Structure**: create the skill folder and a `SKILL.md` with valid frontmatter. Add reference files, templates or scripts only when needed.
- **Description writing**: write a precise description so the skill is used when it should be, and not otherwise.
- **Testing**: run three levels of checks (structure, triggering and output quality), with ready-to-use commands.
- **Troubleshooting**: diagnose and improve existing skills, including skills that don't trigger.

### Included templates
Four fill-in templates for game projects:
- a game design document
- a development plan
- a bug report
- a testing checklist

For what the skill does **not** do, see [Out of scope](#out-of-scope).

## Quick start

1. Install [Claude Code](https://code.claude.com/docs/en/overview).
2. Copy the skill folder into your project or your personal skills folder (see [Installation](#installation)).
3. Start Claude Code and describe what you want, for example: *"Design a tiny Pong game and give me a milestone plan."*

## Installation

### Requirements
- [Claude Code](https://code.claude.com/docs/en/overview)
- Git

### 1. Clone the repository
```bash
git clone --branch claude/gm-architect-skill-plan-um0c2a https://github.com/jasper666780/-GameMaster-Architect.git
```

### 2. Copy the skill folder
Choose one option.

**Option A: Personal** (available in all your projects)
```bash
mkdir -p ~/.claude/skills
cp -r ./-GameMaster-Architect/.claude/skills/gamemaster-architect ~/.claude/skills/
```

**Option B: Project** (available in one project; commit it to share it with collaborators)
```bash
mkdir -p /path/to/your-project/.claude/skills
cp -r ./-GameMaster-Architect/.claude/skills/gamemaster-architect /path/to/your-project/.claude/skills/
```

> **Tip:** the repository name starts with a hyphen, so use paths such as `./-GameMaster-Architect`, as shown above. A plain `cd -GameMaster-Architect` fails because the shell reads `-G` as an option.

### 3. Check that it works
Start a new Claude Code session. For a project install, start it in that project. Then run:
```
/gamemaster-architect Design a tiny Pong game and give me a milestone plan.
```
Claude should follow the skill's workflow: first a short design, then a milestone plan for you to approve.

### Update or remove
To update, pull the latest version, delete the old copy and copy it again. This example is for a personal install:
```bash
git -C ./-GameMaster-Architect pull
rm -rf ~/.claude/skills/gamemaster-architect
cp -r ./-GameMaster-Architect/.claude/skills/gamemaster-architect ~/.claude/skills/
```
To remove the skill, delete its `gamemaster-architect` folder.

## Usage examples

Claude Code uses the skill automatically when your request matches its description. You can also call it directly with `/gamemaster-architect` followed by your request.

### Function A: Game Development
- "Design a top-down roguelike for Godot and give me a milestone plan."
- "Build a playable browser Breakout-style prototype in plain JavaScript."
- "My Unity player jumps twice when I press space once. Fix it."
- "Add a save/load system to my Pygame project."
- "Review my game's architecture and suggest how to structure states and scenes."

### Function B: Skill Creation
- "Create a skill that writes changelogs from git history."
- "Turn this workflow I keep repeating into a Claude Code skill."
- "Make a reusable skill for generating level-design documents for my game."
- "Review my existing skill. It doesn't trigger when it should."
- "Scaffold a skill for asset naming conventions in my Godot projects."

### Tips
- Start Claude Code in your game's folder so it can inspect the project.
- Name your engine and version if the project files don't make it obvious.
- Expect checkpoints. The skill stops before major or risky changes. Reply to approve, adjust or stop.

## Project structure

```
-GameMaster-Architect/
├── README.md                                 # This page
└── .claude/skills/gamemaster-architect/
    ├── SKILL.md                              # Core rules and the workflows for both functions (always read)
    ├── references/                           # Detailed guides, read only when needed
    │   ├── game-design.md                    # Game workflow, engine detection, checklists, design doc template
    │   └── skill-authoring.md                # Skill authoring, test commands, troubleshooting, SKILL.md starter
    └── templates/                            # Fill-in documents for game projects
        ├── game-design-template.md           # Game design document
        ├── development-plan-template.md      # Milestones, tasks, dependencies, verification
        ├── bug-report-template.md            # Reproduction steps, cause, fix, testing
        └── testing-checklist-template.md     # Functionality, gameplay, UI, performance, regression
```

## Testing and safety

The skill follows three core rules in both functions.

**Safe editing**
- Inspect the relevant files and project structure before editing anything.
- Preserve existing functionality and avoid unnecessary rewrites.
- Change only the files the task needs.

**Approval checkpoints**
- Before major or risky changes, explain what will change and why, then wait for your approval. This covers architecture changes, large refactors, deleting or moving files, new dependencies, and anything outside the agreed scope.

**Honest verification**
- Work in small milestones, testing each change with the best method the stack allows.
- Report failures honestly, with the actual output.
- Never claim something works without verifying it.

### How the skill was tested
- **Structure:** the `SKILL.md` frontmatter parses, and every referenced file exists.
- **Triggering:** 13 prompts were each run in a fresh, non-interactive `claude -p` session, using a scratch copy of the project with file-editing tools disabled.
  - 10 of 10 game development and skill creation prompts invoked the skill.
  - 0 of 3 unrelated prompts (SQL, PDF summary, Express API) invoked it.
  - Each run is a single sample, so results can vary between runs.
- **Templates:** each template was filled in during a test run to confirm it is usable.

To re-run these tests, see section 5 of `references/skill-authoring.md`.

## Troubleshooting

| Problem | What to check |
|---|---|
| The skill isn't available | The folder must be at `.claude/skills/gamemaster-architect/` (project) or `~/.claude/skills/gamemaster-architect/` (personal) and contain `SKILL.md`. Start a new session after copying. |
| Claude doesn't use the skill | Mention the game or skill task clearly, or call it directly with `/gamemaster-architect`. |
| `cd: -G: invalid option` | Use `./-GameMaster-Architect` instead of `-GameMaster-Architect`. |

## Out of scope
- Creating art, music or audio assets (planning and specifying them is fine).
- Publishing or store deployment.
- Non-skill Claude Code extensions (hooks, MCP servers, plugins).

## Future plans

> 🚧 **Planned, not available yet.**

- **AI Specialist Ecosystem**: additional specialist AI workflows may be added in a future version, for example for 3D models, textures, animation, environments and audio. None of these exist in the current skill.

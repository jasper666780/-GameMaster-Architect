# Skill Authoring Workflow

Use this guide for Function B in `SKILL.md`. It expands the five workflow steps there (requirements, structure, author, test, improve). The core rules in `SKILL.md` (project safety, approval checkpoints, verification) apply throughout.

## Contents
1. Requirements
2. Structure and location
3. Writing the description
4. Writing the instructions
5. Testing
6. Troubleshooting and improving an existing skill
7. SKILL.md starter

---

## 1. Requirements

Clarify these before creating any file:

| Question | Why it matters |
|---|---|
| What repeated task should the skill handle? | Defines the scope. One skill should do one coherent job. |
| What would a user type when they need it? Collect 3–5 example prompts. | These become the positive trigger tests. |
| What similar requests should **not** trigger it? Collect 2–3. | These become the negative trigger tests. |
| What does it take as input and produce as output? | Shapes the workflow and the quality checklist. |
| Project skill or personal skill? | Decides the location (Section 2). |
| Does it need reference docs, templates or scripts? | Add supporting files only when needed. |

Checklist:
- [ ] Purpose fits in one sentence.
- [ ] 3–5 positive and 2–3 negative example prompts are written down.
- [ ] Location agreed with the user.

## 2. Structure and location

**Location:**
| Scope | Path | Use when |
|---|---|---|
| Project | `.claude/skills/<name>/SKILL.md` | The skill belongs to one repository; commit it so collaborators get it. |
| Personal | `~/.claude/skills/<name>/SKILL.md` | The user wants it in all their projects. |

To reuse a project skill elsewhere, copy its whole folder into `~/.claude/skills/`.

**Layout:**
```
<name>/
├── SKILL.md        # required: frontmatter + core workflow
├── references/     # optional: detailed guides, read only when needed
├── templates/      # optional: files to copy and fill in
└── scripts/        # optional: helper scripts the skill runs
```

**Frontmatter rules:**
- `name`: lowercase letters, numbers and hyphens only, at most 64 characters, matching the folder name.
- `description`: required, at most 1024 characters (see Section 3).
- Other optional fields exist (for example, to restrict tools). Check the current Claude Code documentation before using them rather than relying on memory.

**Progressive disclosure:** Claude sees only `name` and `description` until the skill is used. It then reads `SKILL.md`, and reads supporting files only when `SKILL.md` points to them. So:
- Keep `SKILL.md` short (roughly under 300 lines): rules and the core workflow.
- Move long detail, examples and templates into supporting files.
- Point to each supporting file from the place in `SKILL.md` where it is needed, by relative path (e.g. `references/topic.md`). An unreferenced file will rarely be read.

## 3. Writing the description

The description is what makes the skill trigger, so it matters most.

Include:
- **What** the skill does, in plain words.
- **When** to use it: the user's likely phrasing, task types, file types and named tools or technologies.
- **When not** to use it, if nearby requests could cause false triggers.

Guidelines:
- Write it in the third person, as a statement ("Creates…", "Use when…").
- Prefer concrete verbs and nouns users actually say ("fix", "debug", "doesn't trigger") over abstract categories.
- Cover every main function. A skill with two functions needs both in the description.
- Do not pad it with keywords. Precision beats length.

Example pattern:
`<What it does>. Use when the user wants to <action 1>, <action 2> or <action 3> involving <domain/tool>. Not for <nearby task it should not handle>.`

## 4. Writing the instructions

In `SKILL.md`:
- Start with the purpose in one or two lines.
- Put non-negotiable rules first, short and imperative.
- Give the workflow as numbered steps, each with a clear outcome.
- State where approval is needed before risky actions.
- State how to verify the result.
- List what is out of scope.

Style:
- Imperative and specific ("Run the tests", not "Tests could be run").
- One instruction per line. Avoid repeating the same rule in several places; state it once and refer to it.
- Use examples only where they remove ambiguity.
- No secrets, credentials or machine-specific absolute paths.

## 5. Testing

Run all three levels. Report the actual results, including failures.

**Level 1: Structure.** From the folder that contains the skill:
```bash
python3 - <<'EOF'
import os, re, yaml
p = "<name>/SKILL.md"            # adjust to the skill's path
t = open(p).read()
fm = yaml.safe_load(re.match(r"^---\n(.*?)\n---\n", t, re.S).group(1))
d = os.path.dirname(p)
print("name ok:", fm["name"] == os.path.basename(d) and bool(re.fullmatch(r"[a-z0-9-]{1,64}", fm["name"])))
print("description length:", len(fm["description"]), "(max 1024)")
print("lines:", len(t.splitlines()))
for ref in set(re.findall(r"`((?:references|templates|scripts)/[^`]+)`", t)):
    print("exists" if os.path.exists(os.path.join(d, ref)) else "MISSING", ref)
EOF
```

**Level 2: Triggering.** Run each example prompt from Section 1 in a fresh, non-interactive session, and check whether the skill was invoked:
```bash
claude -p "<prompt>" --output-format stream-json --verbose --max-turns 2 \
  --disallowedTools "Write Edit Bash NotebookEdit" > out.json
grep -q '"skill":"<name>"' out.json && echo TRIGGERED || echo no
```
- Run it in a scratch copy of the project, with file-editing tools disabled, so the test cannot change real files.
- A run that stops at the turn limit still counts if the skill was invoked first.
- Positive prompts must trigger; negative prompts must not.
- Each run is a single sample. Re-run the full set after any description change.

**Level 3: Output quality.** For each main function, run one realistic prompt end to end and compare the result against a short checklist written from the skill's own workflow (did it follow each step, ask for approval where required, and verify its work?).

Checklist:
- [ ] Structure check passes, with no missing referenced files.
- [ ] All positive prompts trigger; no negative prompts trigger.
- [ ] Each main function produced acceptable output.
- [ ] Results reported honestly, including anything not tested.

## 6. Troubleshooting and improving an existing skill

Inspect first: read `SKILL.md` and its supporting files, and run the Level 1 check before changing anything.

| Symptom | Likely cause | Fix |
|---|---|---|
| Not listed or never available | Wrong location, missing or invalid frontmatter, folder/`name` mismatch | Check the path and run the Level 1 check. |
| Doesn't trigger for some prompts | Description lacks the user's phrasing or one of the functions | Add the missing task or wording to the description; re-test. |
| Triggers for unrelated prompts | Description too broad or vague | Narrow the wording; add a "Not for…" clause; re-test negatives. |
| Triggers but ignores parts of the workflow | Instructions too long, vague or buried | Shorten `SKILL.md`; put rules first; use numbered steps. |
| Never uses a reference file or template | `SKILL.md` doesn't point to it where needed | Add a pointer at the relevant step. |
| Inconsistent results | Conflicting or duplicated instructions | State each rule once; remove contradictions. |

When improving:
- Change one thing at a time and re-run the affected tests.
- Keep the skill's intended behavior unless the user approves a change.
- Show the user the diff and the before/after test results.

## 7. SKILL.md starter

Copy, then replace every `<...>` placeholder. (Avoid `[...]` placeholders in frontmatter: square brackets are YAML list syntax and break parsing.)

```markdown
---
name: <skill-name>
description: <What it does>. Use when the user wants to <action 1>, <action 2> or <action 3> involving <domain/tool>. Not for <nearby task it should not handle>.
---

# <Skill Title>

<One or two lines on the purpose.>

## Rules
- <Non-negotiable rule>
- <Approval needed before: ...>

## Workflow
1. **<Step>**: <what to do and its outcome>
2. **<Step>**: <what to do and its outcome>
3. **Verify**: <how to check the result and report it>

For <detail>, read `references/<file>.md`.

## Out of scope
- <Item>
```

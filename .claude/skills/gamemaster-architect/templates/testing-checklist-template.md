# Testing Checklist: [Game Title] / [Milestone or Change]

> Mark each item: **[x]** Pass, **[!]** Fail (add a note), **[-]** Not applicable, **[ ]** Not run.
> Never mark an item as passed unless it was actually tested.

| Field | Value |
|---|---|
| Date | YYYY-MM-DD |
| Build or commit | |
| Platform / device | |
| Tested by | |
| Method | Automated / Run + manual / Scripted (e.g. browser automation) / Manual steps only |

## 1. Functionality
- [ ] Game launches without errors or relevant warnings.
- [ ] New feature or fix works as described in its "Done when" check.
- [ ] All inputs for affected actions respond correctly.
- [ ] Save / load works (if in scope).
- [ ] Scene or level transitions work.

## 2. Gameplay
- [ ] Core loop works from start to end.
- [ ] Win, lose and end conditions trigger correctly.
- [ ] Restart fully resets game state.
- [ ] Difficulty and progression behave as designed.
- [ ] Edge cases: rapid input, simultaneous inputs, pause/unpause, restart mid-game.

## 3. UI
- [ ] HUD values (score, health, timers) are accurate and readable.
- [ ] Menus can be navigated with every supported input method.
- [ ] No dead ends: every screen has a way back or forward.
- [ ] Layout holds at supported resolutions and window sizes.

## 4. Performance
- [ ] Frame rate is stable during normal play on the target platform.
- [ ] No noticeable stutter during heavy scenes or spawning.
- [ ] Memory does not grow steadily over a long session.
- [ ] Load times are acceptable.

## 5. Regression
- [ ] Previously working features most likely affected by this change still work:
  - [ ]
  - [ ]
- [ ] Existing automated tests pass (if any).
- [ ] No unrelated files were changed.

## Failures

| Item | What happened (actual output) | Severity | Bug report link |
|---|---|---|---|
| | | | |

## Not Tested

| Item | Reason |
|---|---|
| | |

## Summary
- Passed:
- Failed:
- Not run:
- Ready to ship this milestone? Yes / No, because...

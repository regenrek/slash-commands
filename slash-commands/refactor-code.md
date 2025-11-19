---
description: Refactor code with a specific goal, keeping changes isolated
argument-hint: GOAL=<refactoring-goal-description>
---

<refactoring_goal>
$GOAL
</refactoring_goal>

Task: <refactoring_goal>

- Keep the commit isolated to this feature.
- Document but do not fix unrelated problems you find.
- Never add fallbacks/backward-compability/feature flags, we are always build the full new refactored solution.
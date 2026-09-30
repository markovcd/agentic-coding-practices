# Coding practices for an agent

Standing rules, skills and a command that make Claude Code (or any agent that
reads `AGENTS.md`) work in a repository the way a careful engineer would.
Distilled from a large agent-built codebase; everything specific to that
project is gone, and what is left holds in any language.

## Install

Copy the tree into the root of the target repository:

```text
AGENTS.md                     entry point; imports every rule
CLAUDE.md                     one line: @AGENTS.md
docs/agents/rules/*.md        standing rules, loaded every session
docs/glossary.md              template: one word per thing
docs/handoff/README.md        template: out-of-scope issues, one file each
.claude/skills/*/SKILL.md     task know-how, loaded when a task matches
.claude/commands/bughunt.md   /bughunt
.claude/agents/taste-check.md a gut-reaction reviewer
.claude/output-styles/terse.md optional wording style
```

If the repository already has a `CLAUDE.md` or `AGENTS.md`, keep yours and paste
the rule list from this `AGENTS.md` into it; the `@path` lines are what make
Claude Code load the rules.

Then fill in the three blanks in `AGENTS.md`: the gate command, the fast test
command, and where the architecture decisions live.

## Edit before use

- `git-workflow.md` commits straight to `main`. For a team that works through
  pull requests, replace that section with the repo's branch rule; the rest of
  the file (squash unpushed churn, isolate from other sessions) still holds.
- `code-style.md` ends with a short .NET section. Replace it with the target
  language's equivalents or delete it.
- `prose-style.md` asks for American spelling. Pick one spelling and say which.
- `windows-shell` only matters on Windows. Delete it elsewhere.
- To turn on the terse output style, add `"outputStyle": "Terse"` to
  `.claude/settings.json`.

## What is where

| File | Says |
|---|---|
| `rules/git-workflow.md` | Commit subjects, squashing churn, never sweeping up another session's work |
| `rules/code-style.md` | Match the file; failures are values; one place knows a thing; no speculative abstraction |
| `rules/testing.md` | A change carries a test that failed first; names are rules; skips say why |
| `rules/honest-tests.md` | Fix the code, never the test: no weakened assertions, skips or test-only branches |
| `rules/security.md` | Secrets out of everything, input checked at the boundary, no loosened checks |
| `rules/ci-cd.md` | CI runs the gate itself; pinned, least-permission workflows; a release is one script that refuses early and ships signed |
| `rules/saved-data.md` | Versioned formats with upgrade steps, ids that are forever, contracts that only grow |
| `rules/prose-style.md` | Short comments that never narrate history |
| `rules/one-type-per-file.md` | One top-level type per file, named for it |
| `rules/agent-drivability.md` | Every feature checkable without eyes on a window; a hack is a feature request |
| `rules/propose-refactors.md` | Say when code has outgrown its shape, after the task, outside its commit |
| `rules/terminology.md` | Hold the glossary's words, in conversation too |
| `rules/working-habits.md` | Stop when done, report failure plainly, verify cheaply, hold "I don't know" |
| `skills/tests` | Rank a run by duration; a slow test is a defect; scenarios as requirements |
| `skills/dependencies` | Check what is outdated before big work; cheap upgrades now, expensive ones handed off |
| `skills/adrs` | Read them before proposing a refactor; amend or rewrite; number at commit time |
| `skills/changelog` | User-facing only, one terse bullet per feature |
| `skills/docs-in-step` | A change to something the docs describe updates them in the same commit |
| `skills/performance` | Measure without fooling yourself; keep what was ruled out, with numbers |
| `skills/running-the-app` | Drive only the process you started, and mark its window |
| `skills/windows-shell` | Encoding and escape pitfalls that corrupt files on Windows |
| `commands/bughunt.md` | Find bugs as failing tests, fix, file the tests under the rule they broke |
| `agents/taste-check.md` | How the code feels, not whether it is correct |

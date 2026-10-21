# <Project>

<One sentence on what this is.> See [README.md](README.md) for the build and run
commands, `docs/adr/` for the decisions behind the design, and
[docs/glossary.md](docs/glossary.md) for the one word each thing is called by.

- **The gate:** `<one command that restores, builds and runs every test in a clean environment>`. It is the truth; a local run can be green about code it skipped.
- **Fast loop:** `<command that runs one test class or file>`.
- **TODO.md** lists work asked for and not yet started. Add to it when the user says "add to the todo", and take an item off in the commit that lands it. A plan, an audit or an out-of-scope problem too long for a line is written up in [docs/handoff/](docs/handoff/README.md) and linked from its item.

Standing rules are in `docs/agents/rules/`. They apply to nearly every task, so read all of them before starting; Claude Code imports them through the paths below:

- @docs/agents/rules/git-workflow.md — declarative commit subjects, squash unpushed churn, never sweep up another session's uncommitted work.
- @docs/agents/rules/code-style.md — new code is indistinguishable from the file it lands in; failures are values; one place knows a thing.
- @docs/agents/rules/testing.md — a behavior change lands with a test that failed before it; test names state the rule.
- @docs/agents/rules/honest-tests.md — a failing test is fixed by fixing the code, never by weakening, skipping or special-casing the test.
- @docs/agents/rules/security.md — secrets stay out of everything; a check is never loosened to make something work.
- @docs/agents/rules/ci-cd.md — land small and often, a red mainline is fixed before anything else, and the mainline is always releasable.
- @docs/agents/rules/one-type-per-file.md — one top-level type per file, named for it; split a file that holds several whenever a change touches it.
- @docs/agents/rules/prose-style.md — succinct comments that never narrate history.
- @docs/agents/rules/terminology.md — when the user says a word the glossary rules out, correct it in one line.
- @docs/agents/rules/agent-drivability.md — drivability by an agent comes first; a hack needed to get something done is a feature to propose.
- @docs/agents/rules/propose-refactors.md — code that has gone untidy or ballooned is a refactor to propose, after the task and outside its commit.
- @docs/agents/rules/working-habits.md — stop when the work is done, report failure plainly, verify at the cheapest level that proves it.

Rules that bind only one kind of change are skills in `.claude/skills/`, loaded when their description matches: `pipeline` (workflows, the gate, a release), `saved-data` (anything saved or passed between versions) and `outside-input` (code that reads input from outside). They are as binding as the rules above. Every other skill there is know-how for one task; its name and description are visible every session, so it is not listed here.

Commands in `.claude/commands/` are run by name: `/bughunt` hunts for bugs, confirms each as a failing test, fixes it, and files the test where it belongs. `/mutate` plants plausible bugs in the lines a commit changed, writes a test for each one the suite missed, and logs the score for `scripts/mutation-score.py`. `/burn` spends spare tokens on the quality or stability chore in its list that ran longest ago, worst hack first, and logs it in `docs/agents/burn-log.tsv`.

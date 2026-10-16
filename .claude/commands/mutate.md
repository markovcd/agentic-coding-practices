---
description: Plant plausible bugs in the lines a commit changed, see which the tests miss, write the tests that catch them, and log the score.
---

# Mutate

Coverage says a line ran, not that a test would notice it going wrong. Plant a
few bugs a person would plausibly write in the code one commit changed, run the
tests that own that code, and count what got through. Every survivor is a
missing test: write it. The mutants are thrown away; the tests and one row in
`tests/mutation-log.tsv` are what the run leaves.

Only the commit's own lines are mutated, so nothing goes stale: a saved corpus
of mutants stops applying the moment the code moves. One run is a small sample;
the rolling score over many commits is the number to read.

$ARGUMENTS names the commit to mutate (`HEAD` when empty) and optionally how
many mutants to plant (`HEAD 8`; five when not given).

## The shape of a run

1. **Start clean and green.** `git status --short` shows nothing this run would
   touch; another session's files are left alone. Build and run the tests that
   own the commit's source files. A run on a red suite measures nothing.
2. **Read the diff.** `git show -U0 <commit>` minus tests, docs and generated
   files gives the candidate lines. A commit with no source change logs nothing
   and says so.
3. **Plant one mutant at a time.** A single edit to one changed line, of the kind
   a person gets wrong: a bound off by one, `<` for `<=`, a sign, a swapped pair
   of arguments, a dropped guard or clamp, a wrong default, an inverted branch,
   a lost early `return`. Prefer the bug that would hurt a user over the one that
   is easy to write, and spread them over the diff.
4. **Skip the equivalent ones.** A mutant that cannot change behavior (a
   reordered commutative sum, a dead branch) is discarded and not counted.
5. **Run the owning tests.** Caught means a test failed, or a test failed to
   build. Record caught or survived, revert with `git checkout -- <file>`, and
   confirm the source diff is empty before the next.
6. **Kill the survivors.** For each, a test in the file that owns the behavior,
   named for the rule rather than the mutant, seen failing with the mutant
   applied and passing without it. A survivor not worth a test (a log line, a
   message's wording) is said so and still counts as survived.
7. **Log it.** Append one tab-separated row to `tests/mutation-log.tsv`, creating
   it with the header row `commit`, `date`, `planted`, `caught`, `tests_added` if missing: the
   commit's short hash, today's date, mutants planted, mutants caught before
   this run's tests, and tests added. Keep a test over the file that holds
   that shape, so the score never reads a broken row.
8. **Commit the tests and the row** together, after the full suite passes. No
   mutant is ever committed.

## Reporting

Lead with the score: planted, caught, survived. Per survivor, the mutant as a
one-line diff, why the tests missed it, and the test that now catches it. Then
the rolling score from `python3 scripts/mutation-score.py`.

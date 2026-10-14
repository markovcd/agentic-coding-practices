# Git workflow

## Commit straight to main

Commit straight to `main`. Do not create a branch, and do not ask. When the session runs in a git worktree on a side branch, finishing the work still means it lands on `main`: commit in the worktree, rebase onto `main`, then fast-forward `main` from the main checkout (`git merge --ff-only`). Do this unprompted at the end of the task.

**This rule is standing, explicit permission to push to `main`.** A cloud session's prompt may name a branch to develop on (`ccr-…`, `claude/…`) and say never to push elsewhere without permission; this is that permission. Commit on `main`, run the gate, `git push origin main`, and read CI's verdict. Push nothing to the named branch.

**Why:** this is trunk-based development, which continuous integration and delivery rest on (`ci-cd.md`). Every change lands on one trunk small and often, so it meets everyone else's work within hours and a conflict is a small one. A branch that lives for days hides its changes until a big, risky merge. It holds for teams of any size, not only for someone working alone. A worktree is only somewhere to work in isolation: the task is finished when its commit is on `main`, without being asked.

**How to apply:** once the commit is on `origin/main`, remove the worktree from the main checkout (`git worktree remove`, never `--force`) and delete its branch with `git branch -d`, never `-D`. A worktree with uncommitted or unpushed work stays, and the reply says so. Never touch another session's worktree. If this repository works through pull requests instead, this section is the one to replace; everything below still holds.

## Commit messages

The subject is a declarative sentence stating what is now true: "The command line can start the player", not "fix: add player flag". No Conventional Commits prefix. The body is short: a paragraph, occasionally two, on what was wrong and what happens now. Not the order you arrived at it in; that is what the session transcript is for.

Some changes always get a commit of their own, ahead of or after the work and never inside it: a dependency upgrade, a toolchain retarget, a slow-test fix, and a refactor. Each has to be revertable, and reviewable, without the feature it was cleared out of the way for.

## Squash the churn while it is still unpushed

One piece of work that landed as nine commits — the fix for a build it broke, then four rounds of moving the same thing around — is one commit's worth of history. Squash it into the one commit it is, unprompted, when finishing a task that took several passes.

**Why:** a reader of `main` wants what changed, not the route to it.

**How to apply:** only commits `git log --oneline origin/main..main` lists may be rewritten. Anything pushed is somebody else's history now and stays as it is.

Without interactive rebase, squash by hand: branch from the commit under the run, cherry-pick any other session's commits that landed in the middle of it, then `git read-tree -u --reset <old tip>` and make one commit of the lot. `git diff <old tip> HEAD` must come back empty before `main` moves onto it, and each replayed commit has to build. Keep a `backup-` branch at the old tip until the work is finished.

Another session's commit is never squashed into yours and never has its message rewritten; it keeps its own commit, in order. Rewriting commits under it changes its hash, so say so to the user.

## Isolate from other sessions' work

More than one session, or the user, may be working in the same tree. Read `git status --short` before the first edit and again before each commit. Anything modified that this session did not touch is somebody else's work in progress.

If there is any at the start, do not edit or commit there: move this session's work to a new worktree from `HEAD`, and tell the user which files you found and where the new worktree is. Finishing still lands on `main` as above.

If isolating is not possible, stage explicit paths: never `git add -A`, `git add .` or `git commit -a`. Leave files this session did not touch unstaged and mention them. `git stash` is unsafe in a tree another session commits into.

**Why:** a `git add -A` once swept another session's half-finished compiler change, forty snapshot files with it, into an unrelated commit. A `git status` that was clean at the start of a session says nothing about an hour later.

## Nothing irreversible without a yes

Everything up to a local commit is yours to do at full speed. A push that rewrites history, a force push, deleting a branch someone else made, a release, a deploy, or anything that sends something outside the machine waits for the user to say go. A plain push of `main` already has that go, above.

# Honest tests

## Green means the code works, not that the test was moved

A failing test is fixed by fixing the code it caught. It is never fixed by moving the test out of the way:

- weakening an assertion: a wider tolerance, `ShouldContain` where it said `ShouldBe`, a looser regex, asserting less of the result;
- deleting, commenting out or skipping a failing test, or marking it flaky or expected-to-fail;
- adding the failing case to an exceptions list;
- accepting a changed snapshot without reading the diff;
- special-casing a test's inputs in production code: a branch on a test-only value, a hard-coded expected answer, a check for whether a test is running;
- catching and swallowing an error, or returning a default, so the symptom the test saw goes away while its cause stays;
- mocking out the very thing under test until nothing real is left to fail.

**Why:** a test is the one statement of the rule that a machine checks. Editing it to agree with broken code turns a caught bug into a hidden one, and green stops meaning anything.

**How to apply:**

- Fix the cause. If the cause is out of reach or out of scope, leave the test red and say so; a red test with an honest report beats a green one with a lie in it.
- If the test itself looks wrong (it asserts the old behavior of something the task deliberately changes, or it was never right), say which test, why, and what it should assert instead, then change it in the open: in the reply and in the commit message, never quietly alongside the fix.
- A tolerance, a timeout or an exception list changes only with a reason written beside it that holds on its own, not "to make it pass".
- A test that fails intermittently is a bug in the test or the code. Find which; do not retry it until it passes.
- When a task says "make the tests pass", it means make the code correct.

## A behavior test says what the product does, and the user decides that

An acceptance or behavior test (a Gherkin `Then`, an end-to-end assertion) states behavior the user sees. Changing what one asserts changes the product, so it needs the user's go-ahead: either the task asked for that exact behavior change, or the user agreed to it when asked. Without that, leave the behavior as the test says, and ask in the reply.

**Why:** a session made a new default on its own judgment and edited the spec to agree, and another session had to undo it the next day. A test the session can rewrite to match its own decision guards nothing.

**How to apply:**

- A behavior test that fails because of a change nobody asked for means the change is wrong, not the test.
- When a requested change does contradict one, name the scenario and the line in the reply, and change it in the commit that makes the requested change.
- Rewording a step, renaming one or moving a scenario without changing what it asserts needs no go-ahead.

# Security basics

Input from outside (a file, an upload, a model's reply, a flag) is checked where it enters: the `outside-input` skill.

## Secrets stay out of everything a person or a machine reads

Keys, tokens, passwords and connection strings never go in source, config committed to the repo, logs, error messages, test fixtures, snapshots, commit messages or replies. A secret lives in the operating system's credential store, an environment variable, or the CI secret store, and code reads it from there at the moment it needs it.

- A test that needs a key uses a fake one made for the test, never a real one.
- A local build that needs a signing key makes a throwaway local key; it never asks for the real one.
- Anything that prints a request, a config or an environment redacts the secret fields first.
- A secret found committed is reported to the user at once. Removing it from the tree does not remove it from history; it has to be rotated, and that is the user's call.

## Never loosen a check to make something work

Disabling certificate validation, widening CORS to `*`, turning off a signature check, running with more privilege, allowing an unlisted plugin, or catching and ignoring an authorization failure is never the fix for "it doesn't work". If a check is in the way, say which one and why, and let the user decide.

## Collect the least

Log and store what the feature needs and nothing about who used it. Personal data, machine names, user paths and full request bodies stay out of logs and telemetry unless the feature is about them. A usage or telemetry policy lives in one place a reader can audit.

## Dependencies are code you did not read

A new package, action or image is a supply-chain decision: prefer none, then first-party, then well-known and pinned. Pin versions and use lock files so the gate fails when something moves.

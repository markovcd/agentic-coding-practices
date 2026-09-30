# Glossary

One word for each thing, and the same word everywhere a person reads: the UI,
the docs, ADRs, comments, test names and commit messages.

- **Say** is the word. Use it in prose, whatever the code calls the thing.
- **Code** is the name in source. Where it differs from **Say**, it differs
  because renaming it would break something persisted or public (a file
  format, an API). A comment on it still uses **Say**.
- **Not** lists words that must not stand for the thing, and why where it
  helps.

Prefer the domain's words over the framework's.

| Say | Means | Code | Not |
|---|---|---|---|
| **example** | Replace this row with the project's first term. | `Example` | sample, demo |
| **assistant** | The model the product talks to, if it has one. | | agent (kept for whoever is building the product) |

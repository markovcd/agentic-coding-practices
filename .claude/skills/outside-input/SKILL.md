---
name: outside-input
description: Use when writing code that reads input from outside - a file, an archive, an upload, a request, a model's reply, a plugin or a command-line flag - sizes, numbers, paths, text and code are checked once, at the door.
---

# Outside input

## Input from outside is checked where it enters

A file, a request, a command-line argument, a model's reply, an archive or a plugin is hostile until checked. Check it once, at the boundary, then trust the checked value inside:

- **Sizes.** A length read from a file or a header is not an allocation. Cap it against what the input could actually hold before allocating.
- **Numbers.** Anything a user types that the program multiplies or indexes by gets a ceiling. Overflow is checked, not assumed away.
- **Paths.** A name from an archive, an upload or a request is resolved and checked to stay inside its intended folder before anything is written (zip-slip, `../`).
- **Text.** User or model text reaching a shell, a query, HTML or a file name is escaped or passed as a parameter, never concatenated. Parsers of untrusted text never throw on bad input; they report.
- **Code.** A plugin, a script or a package loads only if someone said yes to it, and only while it is the thing they said yes to (a hash, a signature).

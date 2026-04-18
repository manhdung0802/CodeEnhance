# Coding Standards — Workspace Baseline

These rules apply to every task in this workspace.
They are designed to compensate for weaknesses common in smaller or less capable models:
poor code quality, silent failures, hallucinated APIs, and unsafe patterns.

## Naming and scope

- Never shadow or reassign built-in / reserved names (`std`, `errno`, `main`, etc.).
- Use descriptive names. Single-letter identifiers are acceptable only for loop indices (`i`, `j`, `k`).
- Prefer `const` / `constexpr` over mutable bindings when the value never changes.

## Algorithm and data structure choice

- Prefer standard-library sort, search, and data-structure implementations over hand-rolled alternatives.
- When you choose a non-trivial algorithm, state its time and space complexity inline.
- Do not reimplement what the standard library already provides.

## Resource management

- Always use the language's idiomatic resource-management construct:
  - C → `goto` cleanup labels or careful manual management
  - C++ → RAII / smart pointers (`std::unique_ptr`, `std::shared_ptr`)
- File descriptors, sockets, database connections, and locks must be released on all exit paths — success and error.

## Asynchronous and I/O patterns

- Prefer async / non-blocking I/O (e.g., `epoll`, `io_uring`, `select/poll`) wherever the system supports it.
- Never issue synchronous block network calls that hang the main thread or event loop.
- Do not use deprecated Linux APIs (e.g., prefer `epoll` over `select` for high performance).

## Error handling

- Never swallow errors silently. Ignoring return values of standard library functions (like `read`, `write`, `malloc`) or an empty `catch (...) {}` is always wrong unless explicitly justified with a comment.
- Always catch the most specific `std::exception` / error type available in C++, or properly check `errno` in C.
- Include actionable context in error messages: what operation failed, with what values, and why.
- Validate inputs at system boundaries (public functions, API handlers, CLI entry points). Do not re-validate deep inside pure logic.

## Uncertainty

- If you are not certain that a function, module, or API exists, say so explicitly — do not fabricate a signature.
- If behavior is version-dependent, state the version assumption.
- Prefer well-documented idioms over custom implementations when both achieve the same goal.

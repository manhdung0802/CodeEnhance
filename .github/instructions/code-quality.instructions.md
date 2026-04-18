---
description: "Use when writing, reviewing, or generating code in any language. Enforces language-agnostic code quality: naming, error handling, resource management, algorithm choice, type safety, and anti-pattern avoidance."
applyTo: "**"
---

# Language-Agnostic Code Quality Rules

## Anti-patterns — never do these

- **Built-in shadowing**: Do not reassign or shadow built-in / reserved names or standard library symbols
  (`std`, `malloc`, `free`, `errno`, etc.).
- **Silent failure**: Do not use empty `catch (...) {}` or ignore system call return values (`(void)read(...)`) without an explicit comment explaining the deliberate discard.
- **Resource leaks**: Do not open files, sockets, or connections without a guaranteed cleanup path on both success and error exits.
- **Blocking I/O on the main thread**: Do not issue synchronous network requests that block the main event loop in networked applications.
- **Deprecated APIs**: Do not use APIs the language/runtime has deprecated — check the standard library first.
- **Reinventing standard library**: Do not reimplement sort, search, or hash when a standard library function exists (e.g. `std::sort`).
- **Mutable when constant**: Do not declare a variable as mutable when the value never changes after initialization.

## Required practices

- **Idiomatic resource management**: Use RAII/smart-pointers (`std::unique_ptr`, `std::shared_ptr`) in C++ or careful manual management with `goto` cleanup labels in C.
- **Specific exceptions**: Catch the narrowest exception/error type possible; include the operation and relevant values in the message. Check `errno` immediately after system calls.
- **Type information**: Prefer strongly typed constructs; do not use `void*` unless truly required by a C API.
- **Complexity annotation**: When using a non-trivial algorithm, annotate its time and space complexity with a short inline comment.
- **Immutable by default**: Prefer `const` / `constexpr` bindings unless mutation is explicitly needed.

## Naming

- Descriptive names for all identifiers; single-letter names are only acceptable as loop indices (`i`, `j`, `k`).
- Functions should have verb names (`calculate`, `fetch`, `validate`); Boolean variables should read as predicates (`isReady`, `hasError`).
- Do not abbreviate unless the abbreviation is universally understood in the domain (e.g., `ctx`, `req`, `res`, `url`).

## On uncertainty

- If you are not certain an API or function exists, state that explicitly rather than guessing.
- If behavior is version-dependent, declare the version assumption inline.
- Prefer documented, idiomatic patterns over bespoke implementations.

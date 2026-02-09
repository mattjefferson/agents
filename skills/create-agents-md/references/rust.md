<!-- Source: Rust style and API guideline conventions -->

# Rust Style Guide Summary

This document summarizes practical Rust conventions for consistent, idiomatic code.

## Tooling Defaults
- **Package Manager:** `cargo`
- **Lint:** `cargo clippy --all-targets --all-features`
- **Format:** `cargo fmt`
- **Test:** `cargo test`

## 1. Tooling
- **Formatting:** Run `rustfmt` (typically `cargo fmt`) on all Rust code.
- **Linting:** Run `clippy` (typically `cargo clippy --all-targets --all-features`) and address warnings.
- **Testing:** Run `cargo test` for affected crates/modules before finalizing.

## 2. Naming
- **Types/Traits/Enums:** `UpperCamelCase`.
- **Functions/Methods/Variables/Modules:** `snake_case`.
- **Constants/Statics:** `SCREAMING_SNAKE_CASE`.
- **Crate names:** short, lowercase, `snake_case` when needed.

## 3. Error Handling
- Prefer `Result<T, E>` over panics for expected failure paths.
- Use `?` to propagate errors instead of manual match boilerplate.
- Reserve `panic!` for unrecoverable invariants/bugs.
- Include clear context when converting or wrapping errors.

## 4. Ownership and Borrowing
- Minimize cloning; prefer borrowing (`&T`, `&mut T`) where possible.
- Keep lifetimes simple by favoring owned return values unless borrowing is clearly better.
- Avoid needless `mut`; make mutability explicit and minimal.

## 5. APIs and Types
- Model invalid states out of existence with enums/newtypes.
- Prefer iterators and combinators over index-heavy loops where readable.
- Keep functions focused; extract helpers instead of large monolithic blocks.
- Derive common traits (`Debug`, `Clone`, `Eq`, `PartialEq`) when appropriate.

## 6. Documentation and Comments
- Add rustdoc comments (`///`) for public APIs and non-obvious behavior.
- Include examples for externally consumed APIs when useful.
- Keep comments for intent and tradeoffs, not obvious mechanics.

## 7. Project Commands
- Build: `cargo build`
- Format: `cargo fmt`
- Lint: `cargo clippy --all-targets --all-features`
- Test: `cargo test`

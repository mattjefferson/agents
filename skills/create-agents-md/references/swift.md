# Swift Style Guide Summary

This document summarizes practical Swift conventions for readable, idiomatic code.

## Tooling Defaults
- **Package Manager:** `SwiftPM`
- **Lint:** `SwiftLint`
- **Format:** `swift-format`
- **Test:** `swift test` (or `xcodebuild test` for Xcode projects)

## 1. Tooling
- **Formatting:** Use `swift-format` or project formatter settings consistently.
- **Linting:** Use `SwiftLint` where configured and resolve warnings in changed code.
- **Build/Test:** Validate with project commands (e.g., `swift test` or `xcodebuild test`).

## 2. Naming
- **Types/Protocols/Enums:** `UpperCamelCase`.
- **Functions/Methods/Properties/Variables:** `lowerCamelCase`.
- **Enum cases:** `lowerCamelCase`.
- **Acronyms:** Follow Swift conventions (e.g., `url`, `httpStatusCode`).

## 3. API Design
- Write method names that read as natural phrases at call sites.
- Prefer value types (`struct`) unless reference semantics are required.
- Use protocols to define behavior boundaries and improve testability.
- Keep APIs small and focused; avoid overloading with unclear semantics.

## 4. Optionals and Errors
- Avoid force unwraps (`!`) unless invariants make it provably safe.
- Prefer `guard` for early exits and flatter control flow.
- Use `throws` for recoverable errors; use `Result` when async/composition benefits from it.
- Provide clear error types and user-readable messages where relevant.

## 5. Control Flow and Readability
- Keep functions short and single-purpose.
- Prefer `guard` and early returns over deep nesting.
- Use `switch` exhaustively, especially for enums.
- Keep comments for intent/tradeoffs, not obvious mechanics.

## 6. Concurrency
- Prefer structured concurrency (`async`/`await`, `Task`) over ad-hoc threading.
- Mark types/methods with actor isolation where needed to protect shared mutable state.
- Keep UI updates on the main actor when required.

## 7. Project Commands
- SwiftPM test: `swift test`
- Xcode build/test: `xcodebuild -scheme <Scheme> test`

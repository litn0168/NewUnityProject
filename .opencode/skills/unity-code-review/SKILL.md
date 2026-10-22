---
name: unity-code-review
description: Review Unity C# code for bugs, lifecycle issues, performance, serialized data compatibility, and maintainability.
---

# Unity C# Code Review

When reviewing or modifying a Unity project:

1. Inspect the existing codebase and follow its conventions.
2. Check MonoBehaviour lifecycle methods and component dependencies.
3. Check null references, object destruction, event subscriptions, and coroutine lifetimes.
4. Check unnecessary allocations and expensive operations in Update.
5. Preserve serialized field names and types whenever possible to avoid breaking Inspector data.
6. Do not rename public APIs or serialized fields without a clear reason.
7. Avoid modifying Library, Temp, Obj, or generated files.
8. Do not claim a change compiles or works in Play Mode unless it has been verified.
9. Report findings with file paths, explain why each issue matters, and suggest concrete fixes.
10. When implementing a feature, inspect existing architecture before introducing new classes or dependencies.

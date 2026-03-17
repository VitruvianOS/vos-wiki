---
title: "Coding Guidelines"
weight: 1
---

# Coding Guidelines

These are the conventions used across the Vitruvian codebase. Consistency matters more than personal preference — when touching existing code, match the surrounding style.

## Indentation

Use **tabs**, not spaces. A tab represents 4 characters of visual width.

## Braces

Opening braces go on their own line for functions and class definitions. Short control flow bodies may keep the brace on the same line.

```cpp
void
NodeMonitor::HandleEvent(uint32 opcode)
{
    if (opcode == B_ENTRY_CREATED) {
        NotifyListeners();
    }
}
```

Function return types go on the line above the function name:

```cpp
status_t
NodeMonitor::StartWatching(const node_ref* ref)
{
    ...
}
```

## Naming

| Kind | Style | Example |
|------|-------|---------|
| Classes and structs | `UpperCamelCase` | `NodeMonitor` |
| Methods and functions | `UpperCamelCase` | `StartWatching()` |
| Member variables | `fUpperCamelCase` | `fListenerCount` |
| Local variables | `lowerCamelCase` | `nodeRef` |
| Parameters | `lowerCamelCase` | `maxCount` |
| Constants | `kUpperCamelCase` | `kMaxListeners` |
| Macros | `ALL_CAPS` | `B_ENTRY_CREATED` |

## Error Handling

Functions that can fail return `status_t`. Do not use C++ exceptions anywhere in the codebase.

```cpp
status_t
MyClass::Init()
{
    fData = new(std::nothrow) DataBuffer();
    if (fData == nullptr)
        return B_NO_MEMORY;

    return B_OK;
}
```

Objects that can fail to initialize expose an `InitCheck()` method returning `status_t`. Callers must check it before using the object.

Never silently ignore error return values.

## Classes

- Destructors of non-final base classes must be `virtual`.
- Prefer `std::nothrow` `new` over plain `new` — check for `nullptr` instead of catching `std::bad_alloc`.
- Keep the public interface minimal. Implementation details are private.

## Comments

Explain **why**, not what. If the code needs a comment to explain what it does, consider making the code clearer first.

```cpp
// Expand frame by 1px so it renders correctly at 12pt font size
iconRect.InsetBy(-1, -1);
```

Use `//` for single-line comments. Use `/* */` for block comments in C files.

## File Headers

All new files must include the appropriate license header for the subsystem they belong to. Kernel and system components use MIT or GPL headers; Tracker and Deskbar code uses the Open Tracker License. When in doubt, ask on [Telegram](https://t.me/vitruvian_official_chat).

## General

- No trailing whitespace.
- One blank line between method definitions.
- Avoid deeply nested logic — early returns keep code flat and readable.
- Keep functions short and focused. If a function needs a comment explaining its sections, it should probably be split.

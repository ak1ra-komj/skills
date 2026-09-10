---
urls:
  - https://frostming.com/posts/2023/error-handling/
  - https://blog.yanli.one/ideas-about-exception-catch
  - https://blog.miguelgrinberg.com/post/the-ultimate-guide-to-error-handling-in-python
---

# Error Handling

## Goals

- Make failure paths explicit and understandable.
- Catch exceptions only where you can recover.
- Preserve error context for debugging.

## Guidance

- Prefer EAFP where it improves clarity, but keep failure scope tight.
- Catch specific exceptions; avoid blanket catches.
- Batch tasks can isolate per-item failures; pipelines often stop early.
- When re-raising, keep the original context.

## Example

```python
try:
    config = json.loads(raw)
except json.JSONDecodeError as exc:
    raise InvalidConfig(path) from exc
```

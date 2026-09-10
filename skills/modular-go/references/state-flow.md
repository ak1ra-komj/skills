---
urls:
  - https://go.dev/blog/context
  - https://go.dev/doc/effective_go#concurrency
---

# State Flow

## Goals

- Keep behavior deterministic and easy to reason about.
- Make state ownership and lifecycle explicit.

## Guidance

- Prefer stateless functions for transformation, validation, and composition.
- Pass dependencies and data explicitly; avoid global mutable state.
- If state is unavoidable, model one-way transitions (init → advance → finalize) and prevent backward transitions.
- Use `XXXManager` only for lifecycle-managed collections; expose operations by stable IDs.
- Guard manager-owned mutable registries with explicit synchronization and narrow lock scope.
- Pair shared resource acquisition with deterministic release paths.
- Prefer shutdown through `context.Context`; expose `Close()` only when external contracts require it.

---
urls:
  - https://go.dev/blog/package-names
  - https://go.dev/doc/effective_go
---

# Module Boundary

## Goals

- Keep each package easy to explain in one sentence.
- Minimize change impact by shrinking the public surface.

## Guidance

- Keep non-entry helpers and assembly details unexported.
- Export additional symbols only as stable domain contracts.
- Prefer deep modules with narrow APIs over wide public surfaces.
- Keep interfaces behavior-focused; hide concrete infrastructure behind constructors and internal wiring.
- For wide interfaces, define them at the package entry surface (for example, `api.go`) and comment every method with behavior and caller expectations.
- Extract reusable pure helpers into domain-named `xxxutil` packages; avoid generic names like `common`.
- Keep `xxxutil` stateless.
- Use a dedicated `Builder` when parameters are complex or need cross-field validation.
- Inline adapters when they have a single consumer; indirection should earn its keep.

## Single Responsibility Examples

- **Initialization vs. serving**: Wiring dependencies and starting background loops belongs separate from request handling.
- **Protocol translation vs. domain logic**: gRPC/HTTP handlers translate wire formats; business decisions live in injected domain services.
- **State lifecycle vs. business operations**: A manager that tracks resources by ID is distinct from the logic that operates on those resources.
- **Reusable utility vs. domain package**: Pure, stateless helpers shared across packages belong in a dedicated `xxxutil`; domain-aware logic stays in its owning package.

## Deep vs Wide Interfaces

- Deep interfaces: few operations, business semantics, stable over time.
- Wide interfaces: many operations, adapter semantics, integration detail exposure.
- Keep deep interfaces in core packages and wide interfaces at boundaries.

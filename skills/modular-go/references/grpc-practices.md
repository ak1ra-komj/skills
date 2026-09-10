---
urls:
  - https://grpc.io/docs/languages/go/basics/
  - https://go.dev/blog/context
---

# gRPC Practices

## Goals

- Keep transport concerns isolated from domain logic.
- Make gRPC services easy to test without starting a real server.

## Guidance

- gRPC service impls are translators: unmarshal request, call domain service, marshal response.
- Keep all business decisions in injected `XXXManager` or domain `XXXHandler` dependencies, not in the gRPC service itself.
- Convert domain errors to gRPC status codes at the handler boundary in one centralized place.
- Use `context.Context` from the incoming RPC for cancellation and deadline propagation; do not create detached contexts.
- When handlers accumulate enough complexity, extract them into a dedicated package alongside the server definition.
- Keep server initialization and the gRPC service implementation in separate packages: the server package handles dependency injection and initialization; the service package handles request/response translation.
- Register services through a constructor that accepts domain dependencies explicitly; avoid global state or init-time registration.

## Structuring a gRPC Package

- `server.go`: Server constructor, listener setup, graceful shutdown.
- `service.go`: Handler methods implementing the generated interface.
- Keep proto-generated code in its own module or a `proto/` subdirectory; do not mix generated and hand-written code.

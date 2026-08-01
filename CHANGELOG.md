# Turtle Ecosystem Changelog

All notable changes to the Turtle ecosystem will be documented in this file.

## [v0.2.0] - 2026-08-01

### Added
- **Ecosystem Architecture**: Decoupled applications into micro-repositories under `C:\turtle\`.
- **`turtle-meta`**: Introduced private management repository for master roadmaps, agent planning, and environment orchestration.
- **`turtle-docs`**: Introduced public repository for architecture specifications, API references, and open roadmaps.
- **Messaging Microservice (`message`)**: Initialized Go GraphQL backend and Gleam WebSocket server.
- **Messaging Clients**: Scaffolded `messager` (Flutter) and `messager-web` (Angular 22).
- **Caddy Modularization**: Extracted Turtle proxy routes into `C:\turtle\caddy\Caddyfile`.

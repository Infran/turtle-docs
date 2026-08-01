# Turtle Ecosystem Architecture Overview

The Turtle platform uses a decoupled micro-service and micro-frontend architecture behind a reverse proxy (Caddy).

```mermaid
graph TD
    Client[Browser / User] --> |HTTPS| Caddy[Caddy Reverse Proxy]
    
    subgraph Reverse Proxy & Gateway
        Caddy -->|finances.infran.dev.br| FinancesWeb[finances-web - Angular 22 :8081]
        Caddy -->|turtle.infran.dev.br/api| CoreGo[turtle - Go API :8080]
        Caddy -->|message.infran.dev.br/query| MsgGo[message - Go GraphQL API :8089]
        Caddy -->|message.infran.dev.br/ws| MsgGleam[message - Gleam WebSockets :8090]
        Caddy -->|releases/*| RelServer[release-server - Rust Axum :18182]
    end

    subgraph Data Stores
        CoreGo --> PostgreSQL[(PostgreSQL 16)]
        MsgGo --> PostgreSQL
        MsgGo --> Valkey[(Valkey Cache & Pub/Sub)]
        MsgGleam --> Valkey
    end

    subgraph Mobile & Web Frontends
        FinancesWeb --> CoreGo
        MessagerMobile[messager - Flutter Mobile] --> MsgGo
        MessagerMobile --> MsgGleam
        MessagerWeb[messager-web - Angular 22] --> MsgGo
        MessagerWeb --> MsgGleam
    end

    subgraph Dev Tools
        CLI[cli - Turtle CLI & GUI] --> RelServer
        CLI --> CoreGo
    end
```

## Core Components
- **`turtle`**: Core Go financial API backend.
- **`finances-web`**: Dedicated Angular 22 financial management web application.
- **`message`**: Dedicated messaging service with GraphQL (Go) and Gleam (Mist/Wisp) WebSockets fanout via Valkey.
- **`messager` & `messager-web`**: Specialized chat clients built in Flutter and Angular 22.
- **`release-server`**: High-performance Rust binary server managing client releases.
- **`cli`**: Go Cobra CLI and Flutter GUI dashboard for ecosystem administration.
- **`turtle-i18n`**: Standalone localization and translation package.

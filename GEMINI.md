# Gemini CLI Context: auto-novel

This project is a light novel machine translation and sharing platform. It is a monorepo consisting of a web frontend, a Kotlin backend server, and a Node.js crawler service.

## Project Overview

- **Web Frontend (`/web`)**: Built with Vue 3, TypeScript, Vite, and Naive UI.
- **Backend Server (`/server`)**: Built with Kotlin, Ktor, and Koin. It uses MongoDB for primary storage, Elasticsearch for searching, and Redis for caching.
- **Crawler Service (`/crawler`)**: A Node.js service for scraping novels, using Cheerio for parsing and SQLite for proxy management.
- **Infrastructure**: Orchestrated via Docker Compose, with persistent data stored in the `/data` directory.

## Building and Running

### Full Stack (Docker)

To start the entire stack in the background:

```bash
# 1. Prepare data directories
mkdir -p -m 777 ./data/es/data ./data/es/plugins

# 2. Start services
docker compose up -d
```

The web service will be accessible at `http://localhost:8765`.

### Development Mode

To enable debug ports for databases (Mongo: 5001, ES: 5002, Redis: 5003):

```bash
export COMPOSE_FILE="docker-compose.yml:docker-compose.debug.yml"
docker compose up -d
```

### Component-Specific Development

#### Web Frontend
```bash
cd web
pnpm install
pnpm dev          # Connects to production API (remote)
pnpm dev:local    # Connects to local Docker API
pnpm build        # Production build
```

#### Crawler Service
```bash
cd crawler
pnpm install
pnpm dev          # Build in watch mode
pnpm test         # Run vitest
```

#### Backend Server
- Recommended IDE: IntelliJ IDEA.
- Uses Gradle for dependency management.
- Run tests via `./gradlew test` or using a Kotest IDE plugin.

## Development Conventions

- **Package Manager**: Use `pnpm` for all Node.js related tasks.
- **Coding Style**:
    - **JS/TS**: Adhere to ESLint and Prettier configurations found in the respective directories.
    - **Kotlin**: Follow standard Kotlin coding conventions.
- **Testing**:
    - Frontend/Crawler: Use `vitest`.
    - Backend: Use `kotest`.
- **Git Workflow**:
    - Keep Pull Requests focused on a single change.
    - Discuss major features via Issues before implementation.
- **Environment Variables**: Configure via a `.env` file in the root directory (see `README.md` for template).

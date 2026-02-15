# Session Progress: auto-novel

## Completed Tasks
- **System Analysis**:
    - Analyzed the translation workflow: Web Frontend orchestrates AI calls (Sakura/GPT), while the Backend provides text segments via `translate-v2` APIs.
    - Verified the Crawler logic: The Backend uses built-in providers (Ktor-based) to fetch Japanese text, while the separate Crawler service provides proxy and auxiliary scraping capabilities.
- **Local Development Auth Bypass**:
    - Modified `server/src/main/kotlin/api/plugins/Authentication.kt` to introduce `LOCAL_DEV` mode. It bypasses JWT verification and injects a mock "LocalAdmin" user when the environment variable is active.
    - Updated `web/vite.config.ts` and `web/Dockerfile` to allow passing `VITE_API_MODE=local` during the build process, enabling the frontend's no-auth state.
- **Docker Infrastructure Optimization**:
    - Converted `docker-compose.yml` to use local `build` contexts for `web` and `api` (server) services, allowing users to run their own modified code.
    - Fixed Docker build errors related to `COPY` syntax (trailing slashes).
    - Configured `docker compose` to pass build arguments required for the local frontend configuration.

## Mistakes & Lessons Learned
- **Static Build Context**: Initially forgot that Vite environment variables (`VITE_API_MODE`) are injected at **build time**. Setting them in `docker-compose.yml` environment only affects runtime, which is too late for a static SPA. Resolved by adding `ARG` to `Dockerfile` and `args` to `docker-compose.yml`.
- **Ambiguous COPY Syntax**: Attempted to `COPY` multiple files without a trailing slash on the destination directory, causing a Docker build failure. Corrected to `COPY src/ dest/` format.
- **Service Clarification**: Corrected the assumption about the `crawler` image; the project primarily relies on the Kotlin server's internal providers for standard web novel fetching, though a Node.js crawler exists as a sibling service.

## Current State
- The stack is fully runnable locally with built-in images.
- Authentication is automatically bypassed in `LOCAL_DEV` mode, granting full Admin access to translation tools.
- Users can import novels via URL and translate them using local or shared Sakura AI endpoints without an account.
- The web service remains accessible at `http://localhost:8765`.

# Bootstrap Assessment Report — AI Course Knowledge & Script Generation Platform

This assessment report evaluates the readiness, structural health, and next recommended actions for onboarding the **AI-powered Content Course Knowledge and Script Generation** project into the Universal AI Software Factory.

---

## 1. Overall Bootstrap Status

**Status:** `BOOTSTRAPPED` (Factory Infrastructure Ready; Application Foundation Pending)

The Factory multi-agent infrastructure, governance rules, and execution workflows are fully intact and functional. The application itself is at **Stage 0 (Greenfield)**, ready for formal requirements discovery and architectural design.

---

## 2. Detected Stack & Project Conventions

### Intended Application Stack
- **Web Framework:** Next.js (App Router)
- **Language:** TypeScript
- **Database:** MySQL
- **ORM:** Prisma
- **Browser Automation:** Playwright (for authenticated session handling, course traversal, and lesson transcript collection)
- **AI Engine:** Google Gemini (isolated behind an extensible AI Provider Abstraction)

### Existing Conventions
- **Governance:** [`AGENTS.md`](file:///c:/Users/tinhn/Desktop/universal-ai-software-factory/AGENTS.md) acts as the primary source of truth for agent behavior, Git safety, database integrity, and secret prevention.
- **Workflow Orchestration:** [`docs/orchestration.md`](file:///c:/Users/tinhn/Desktop/universal-ai-software-factory/docs/orchestration.md) dictates the phase gate flow (Understand → Research/Discover → Spec → Architecture → Implement → Verify → Finish).
- **Subagent Permissions:** All six worker agents have `enable_mcp_tools: false` declared in their frontmatter, preserving the Main Agent as the singular MCP security gateway.
- **Documentation Standards:** Durable artifacts are preserved in `docs/` (`docs/factory/`, `docs/specs/`, `docs/architecture/`, etc.).

---

## 3. Factory Baseline Component Checklist

| Component | Path | Status | Verification Notes |
| :--- | :--- | :---: | :--- |
| **Governance Document** | [`AGENTS.md`](file:///c:/Users/tinhn/Desktop/universal-ai-software-factory/AGENTS.md) | **Present** | Sections 1–20 intact; strict safety & boundaries enforced |
| **Orchestration Policy** | [`docs/orchestration.md`](file:///c:/Users/tinhn/Desktop/universal-ai-software-factory/docs/orchestration.md) | **Present** | Multi-agent gated lifecycle and artifact requirements intact |
| **Worker Subagents** | [`.agents/agents/`](file:///c:/Users/tinhn/Desktop/universal-ai-software-factory/.agents/agents/) | **Present (6/6)** | `researcher`, `product-analyst`, `architect`, `developer`, `reviewer`, `qa` all present with `enable_mcp_tools: false` |
| **Skills Library** | [`.agents/skills/`](file:///c:/Users/tinhn/Desktop/universal-ai-software-factory/.agents/skills/) | **Present** | 39 skills available (including `factory-bootstrap`, `implement-spec`, `tdd`, `domain-modeling`) |
| **Durable Profile** | [`docs/factory/project-profile.md`](file:///c:/Users/tinhn/Desktop/universal-ai-software-factory/docs/factory/project-profile.md) | **Present** | Captures application mission, target stack, and constraints |
| **Bootstrap Report** | [`docs/factory/bootstrap-report.md`](file:///c:/Users/tinhn/Desktop/universal-ai-software-factory/docs/factory/bootstrap-report.md) | **Present** | Current assessment |

---

## 4. Missing Project-Specific Tooling & Scaffolding

Because application development has intentionally not begun, the following project-level runtime files and toolchain components are currently missing:

1. **Root Package Manifest & Lockfile:** `package.json`, package manager setup (`pnpm` or `npm`).
2. **TypeScript Configuration:** `tsconfig.json`.
3. **Database & ORM Configuration:** `prisma/schema.prisma` and Prisma client setup.
4. **Browser Automation Configuration:** `playwright.config.ts` and test/automation runner harness.
5. **Security & Configuration Templates:** `.env.example` defining parameters for MySQL connection strings, Gemini API keys, and session storage configuration.
6. **Linting & Code Formatting:** `.eslintrc.json` / ESLint config and Prettier setup.
7. **Test Framework:** Vitest or Jest for unit and integration testing.

---

## 5. Potential Conflicts & Architectural Risks

1. **Authentication & Session Persistence via Playwright:**
   - Automating user login requires handling session cookies, multi-factor authentication (if applicable), and credential storage without ever writing plaintext passwords to repository files.
   - *Mitigation:* Architecture must design a secure session storage model and headless/interactive login flow (e.g. storage state JSON placed in ignored `.auth/` path).
2. **AI Provider Abstraction Seam:**
   - Direct coupling of Gemini SDK calls across UI or ingestion components would violate modularity rules.
   - *Mitigation:* Must define a clean interface (e.g., `AiProvider` interface with methods for chunk analysis, embedding/knowledge extraction, and script synthesis) with a Gemini adapter implementation.
3. **Data Ingestion & Rate Limiting / Idempotency:**
   - Ingestion of large course hierarchies (modules → lessons → transcripts) could suffer partial network failures or site throttling.
   - *Mitigation:* Prisma schema and ingestion pipeline must support idempotent upserts and resumeable crawler checkpoints.
4. **MCP Access Boundaries:**
   - The subagents must not be granted direct external MCP or database access; database migrations and operations must be executed through controlled local scripts/Prisma CLI or via Main Agent oversight.

---

## 6. Recommended Next Steps

In accordance with the Universal AI Software Factory gated workflow:

1. **Gate 1 — Requirements Discovery & Specification (Phase B & C):**
   - Delegate to `product-analyst` to create the functional specification (`docs/specs/course-knowledge-platform.md`), covering:
     - User authentication & session acquisition flow
     - Course discovery and transcript scraping mechanics
     - Knowledge base data model (Course, Module, Lesson, Transcript, Chunk, Topic)
     - Script generation requirements (prompt templates, tone, audience customization)
     - Acceptance criteria
2. **Gate 2 — Technical Architecture & ADRs (Phase D):**
   - Delegate to `architect` to produce technical contracts (`docs/architecture/system-design.md`):
     - Next.js application architecture & server actions / API routes
     - Playwright scraper worker module & session isolation
     - Prisma schema design for MySQL
     - AI Provider abstraction interface (`GeminiProvider`)
     - Phased implementation sequence
3. **Gate 3 — Scaffolding & Implementation (Phase E):**
   - Delegate to `developer` to initialize Next.js, Prisma, Playwright, and dependencies, followed by TDD implementation of core modules against the approved contracts.
4. **Gate 4 — Verification (Phase F):**
   - Run static analysis via `reviewer` and runtime/automation tests via `qa`.

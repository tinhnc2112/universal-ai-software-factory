# Using the Universal AI Software Factory as a Template

The Universal AI Software Factory is a reusable, domain-neutral software engineering framework designed for Google Antigravity. It equips any software project with a disciplined, gated multi-agent development lifecycle, strictly defined role boundaries, and durable artifact handoffs.

This document describes how to use this repository as a template for new projects or how to adopt its structure into existing codebases.

---

## 1. Onboarding Paths

Choose the adoption path that fits your project scenario:

### Option A — Clone as a Starter Repository (Greenfield Projects)
Use this option when starting a brand new project from scratch.

1. **Clone or Copy the Repository:**
   ```bash
   git clone <universal-ai-software-factory-url> my-new-project
   cd my-new-project
   ```
2. **Reinitialize Git (Optional for Independent History):**
   ```bash
   rm -rf .git
   git init -b main
   ```
3. **Preserve the Factory Core Structure:**
   Ensure the following directories and baseline files remain intact:
   - `AGENTS.md`
   - `.agents/agents/` (the 6 worker agents)
   - `.agents/skills/` (reusable engineering workflows)
   - `docs/orchestration.md`
4. **Trigger Initial Assessment:**
   Open the repository in Google Antigravity and run the bootstrap command in chat:
   ```text
   /factory-bootstrap
   ```
   The Main Agent will assess the workspace, create `docs/factory/project-profile.md` and `docs/factory/bootstrap-report.md`, and confirm the project baseline is ready for specification and development.

---

### Option B — Adopt Factory into an Existing Codebase (Brownfield Projects)
Use this option when introducing the Factory into an existing, active software repository.

1. **Copy Factory Assets into the Target Repository:**
   Copy the following folders and files from the Factory repository to the root of your existing codebase:
   - `AGENTS.md` (reconcile with any existing project guidelines)
   - `docs/orchestration.md`
   - `.agents/agents/` (all six worker subagent definitions)
   - `.agents/skills/` (the reusable skill library)
2. **Preserve Existing Project Conventions:**
   Do not overwrite existing `package.json`, build systems, test suites, or Docker configurations. The Factory is designed to wrap around your existing stack.
3. **Execute Bootstrap Assessment:**
   In Antigravity, invoke:
   ```text
   /factory-bootstrap
   ```
4. **Review the Assessment Report:**
   The Main Agent will inspect the codebase and generate:
   - `docs/factory/project-profile.md` — Detailed breakdown of detected languages, frameworks, test harnesses, and architecture.
   - `docs/factory/bootstrap-report.md` — Gap analysis identifying compatibility status (`BOOTSTRAPPED`, `PARTIALLY_BOOTSTRAPPED`, or `NEEDS_REVIEW`), missing components, and suggested next steps.
5. **Resolve Conflicts (if any):**
   If existing AI instructions (e.g. `CLAUDE.md`, `.cursorrules`) conflict with `AGENTS.md`, align them so `AGENTS.md` acts as the central governing standard while preserving domain-specific guidelines.

---

## 2. First Action in a New or Onboarded Project

The very first action in any new or newly adopted project should always be:

```text
/factory-bootstrap
```

The bootstrap process guarantees that:
- The six worker subagents (`researcher`, `product-analyst`, `architect`, `developer`, `reviewer`, `qa`) are discoverable and active.
- Tool boundaries are verified (`enable_mcp_tools: false` on workers).
- The development lifecycle and verification gates (`docs/orchestration.md`) are established.
- Initial project characteristics are documented cleanly in durable repository artifacts before implementation begins.

---

## 3. Recommended Factory Project Workflow

Once bootstrapped, all feature development and refactoring should proceed through the standard gated lifecycle:

1. **Phase 1 — Discovery:** Main Agent invokes `researcher` and `product-analyst` in parallel to investigate technical dependencies and clarify functional requirements.
2. **Phase 2 — Specification Gate:** Requirements and acceptance criteria are locked in `docs/specs/<feature>.md`.
3. **Phase 3 — Architecture:** `architect` defines API contracts, data models, and implementation breakdown in `docs/architecture/<feature>.md` and `docs/adr/`.
4. **Phase 4 — Implementation:** `developer` implements production code and local unit tests against approved specs and architecture.
5. **Phase 5 — Independent Verification:** `reviewer` (static analysis, security) and `qa` (runtime tests, E2E) verify the feature in parallel.
6. **Phase 6 — Remediation (if needed):** Any failures route back to the responsible subagent for targeted fixes.
7. **Phase 7 — Finish Gate:** Main Agent performs final synthesis and verifies repository cleanliness.

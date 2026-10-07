# Universal AI Software Factory

A reusable, domain-neutral software engineering framework designed for **Google Antigravity**.

The Universal AI Software Factory transforms high-level product requirements into production-quality software through a disciplined, gated multi-agent engineering lifecycle, explicit role boundaries, strict tool isolation, and durable repository handoffs.

---

## 1. System Architecture

```text
                               +-------------------+
                               |       USER        |
                               +---------+---------+
                                         |
                                         v
                         +-------------------------------+
                         |   Main Agent / Orchestrator   |<== [ Browser (Built-in) ]
                         |   (Gatekeeper & Synthesis)   |<== [ GitHub MCP (Gateway Only) ]
                         +---------------+---------------+
                                         |
       +-------------------+-------------+-------------+-------------------+
       |                   |                           |                   |
       v                   v                           v                   v
+--------------+   +-----------------+           +-----------+       +------------+
|  researcher  |   | product-analyst |           |  reviewer |       |     qa     |
| (Fact/Docs)  |   | (Specs/Journeys)|           |  (Static) |       |  (Runtime) |
+--------------+   +-----------------+           +-----------+       +------------+
  (Phase 1 - Parallel Discovery)                   (Phase 5 - Parallel Verification)
       |                   |                           ^                   ^
       +---------+---------+                           |                   |
                 v                                     +---------+---------+
       +-------------------+                                     |
       |     architect     |                                     |
       |  (Technical/ADRs) |                                     |
       +---------+---------+                                     |
                 | (Phase 3 - Architecture)                      |
                 v                                               |
       +-------------------+                                     |
       |     developer     |-------------------------------------+
       | (Code/Unit Tests) | (Phase 4 - Implementation)
       +---------+---------+
                 |
                 + - - - - - - [ MySQL MCP (Optional, Project-Level Only) ]
```

### Key Architectural Boundaries
* **Main Agent as Orchestrator:** The primary Antigravity agent manages the intake, phase transitions, gate evaluation, and final synthesis. It acts as the sole gateway for external orchestration tools.
* **Worker Subagents (`enable_mcp_tools: false`):** All six worker subagents operate strictly within their domain boundaries without direct external MCP access. They consume context passed by the Main Agent or durable repository artifacts.
* **GitHub MCP Gateway:** Configured globally on the Main Agent only, scoped strictly to orchestration toolsets (`repos,issues,pull_requests`) with high-risk operations excluded (`push_files`, `merge_pull_request`, `create_or_update_file`, etc.).
* **MySQL MCP (Optional & Project-Level):** Not globally enabled. Configured per-project in `.agents/mcp_config.json` with read-only principles when required by a specific application.
* **Browser Automation:** Utilizes Antigravity's native browser capabilities directly through the Main Agent / QA verification gates.
* **Infrastructure History:** Legacy Cobalt-specific computer configurations (`.cobalt/README.md`) have been intentionally removed in favor of standard Antigravity workspace primitives and portable VCS baselines.

---

## 2. The Six Specialized Worker Agents

The Factory provides six custom workspace worker agents located in `.agents/agents/<role>/agent.md`:

| Role | Core Responsibilities | Typical Handoff Artifact |
| :--- | :--- | :--- |
| **`researcher`** | Investigates unfamiliar third-party libraries, APIs, external docs, and technical options. Facts and evidence only. | `docs/research/<topic>.md` |
| **`product-analyst`** | Clarifies business requirements, maps user journeys, defines acceptance criteria, and resolves product ambiguity. | `docs/specs/<feature>.md` |
| **`architect`** | Evaluates feasibility, defines module boundaries, API contracts, persistence schemas, and implementation task sequences. | `docs/architecture/<feature>.md`<br>`docs/adr/*.md` |
| **`developer`** | Implements production code and focused unit tests strictly against approved specifications and architecture. | Production source code & unit tests |
| **`reviewer`** | Performs independent static code review, security audits, architecture conformance checks, and dependency hygiene. | `docs/reviews/<feature>.md` |
| **`qa`** | Owns authoritative runtime verification, executes integration and E2E test suites, and verifies acceptance criteria. | `docs/qa/<feature>.md` |

---

## 3. Gated Development Lifecycle

For all non-trivial tasks, the Factory enforces a structured, gated lifecycle documented in [`docs/orchestration.md`](docs/orchestration.md):

1. **Phase A — Intake:** Main Agent evaluates scope, detects constraints, and determines required agents.
2. **Phase B — Parallel Discovery:** `researcher` and `product-analyst` run concurrently to explore external technology and clarify functional requirements.
3. **Phase C — Specification Gate (Gate 1):** Functional specifications and acceptance criteria must be approved before architecture begins.
4. **Phase D — Architecture Gate (Gate 2):** `architect` establishes data models, API contracts, and implementation steps based strictly on verified requirements.
5. **Phase E — Implementation Gate (Gate 3):** `developer` implements source code and local unit tests without silent re-architecting.
6. **Phase F — Independent Verification (Gate 4):** `reviewer` (static/security) and `qa` (runtime/E2E) verify the feature concurrently.
7. **Phase G — Remediation Loop:** Defect findings trigger a targeted remediation cycle back to the responsible agent, followed by re-verification.
8. **Phase H — Finish Gate:** Main Agent performs final synthesis, verifies repository cleanliness, and ensures truth in completion reporting.

---

## 4. Reusable Engineering Skills

Under `.agents/skills/`, the Factory includes a library of battle-tested, reusable engineering skills:
* **`factory-bootstrap`**: Deterministic procedure to assess, profile, and onboard new or existing projects into the Factory.
* **Requirements & Design**: `to-spec`, `domain-modeling`, `codebase-design`, `research`, `grilling`.
* **Implementation & Testing**: `implement`, `implement-spec`, `tdd`, `prototype`, `migrate-to-shoehorn`.
* **Quality & Git Safety**: `code-review`, `diagnosing-bugs`, `git-guardrails-claude-code`, `pr`, `handoff`.

---

## 5. Repository Structure

```text
├── AGENTS.md                                # Master repository authority & rules of engagement
├── README.md                                # Project overview & architectural guide
├── skills-lock.json                         # Skill dependency lockfile
├── .agents/
│   ├── agents/                              # Six specialized worker subagents
│   │   ├── researcher/agent.md
│   │   ├── product-analyst/agent.md
│   │   ├── architect/agent.md
│   │   ├── developer/agent.md
│   │   ├── reviewer/agent.md
│   │   └── qa/agent.md
│   └── skills/                              # Reusable skill catalog (including factory-bootstrap)
└── docs/
    ├── orchestration.md                     # Gated orchestration policy & remediation rules
    ├── factory/                             # Factory bootstrap & template documentation
    │   ├── template-usage.md
    │   ├── project-profile.md               # Generated by /factory-bootstrap
    │   └── bootstrap-report.md              # Generated by /factory-bootstrap
    └── integrations/
        └── mysql-mcp.md                     # Optional project-level MySQL MCP documentation
```

---

## 6. Template Usage & Getting Started

### Using as a Starting Template
You can use this repository as a starter template for greenfield projects or adopt it into an existing brownfield repository. See [`docs/factory/template-usage.md`](docs/factory/template-usage.md) for detailed instructions.

### The First Action in Any Project
Whenever opening a project initialized with or adopting the Factory, run:

```text
/factory-bootstrap
```

The bootstrap skill deterministically:
1. Detects your language, runtime, package manager, and test toolchain.
2. Verifies that all 6 worker agents and governance files are present.
3. Detects any conflicts with existing project conventions.
4. Generates a durable profile at `docs/factory/project-profile.md`.
5. Produces a health status report at `docs/factory/bootstrap-report.md` (`BOOTSTRAPPED`, `PARTIALLY_BOOTSTRAPPED`, or `NEEDS_REVIEW`).

---

## 7. Validation & Baseline Status

* **Smoke-Test Validated:** The complete 7-phase Factory lifecycle (Intake → Discovery → Spec Gate → Architecture → Implementation → Independent Verification → Finish Gate) was smoke-tested end-to-end on an isolated branch with all six subagents executing their respective roles.
* **Security & Boundary Audited:** Global MCP access is constrained to the Main Agent gateway, worker subagents are locked (`enable_mcp_tools: false`), and credentials are strictly kept out of version control.

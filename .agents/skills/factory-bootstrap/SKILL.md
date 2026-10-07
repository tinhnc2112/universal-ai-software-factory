---
name: factory-bootstrap
description: Bootstrap and assess a software project using the Universal AI Software Factory. Use when initializing a new project, onboarding an existing repository, or asking whether the Factory is correctly configured.
---

# Universal AI Software Factory — Bootstrap Skill

This skill defines the deterministic procedure for inspecting, evaluating, and onboarding a software project into the Universal AI Software Factory. It works for new greenfield projects, existing legacy/active codebases, and existing Factory installations.

---

## 1. Core Principles & Safety Rules

The bootstrap skill is strictly conservative, non-destructive, and idempotent:
- **No Destructive Operations:** Never delete existing project instructions, custom agents, skills, tests, build configurations, or production source code.
- **No Unsolicited Modifications:** Do not modify application code, alter database schemas, install package dependencies, or commit/push changes during assessment.
- **Zero Credential Exposure:** Never print, log, or store secrets, passwords, tokens, API keys, private credentials, or sensitive environment variable values.
- **Preserve Established Patterns:** If a project already has established architecture, conventions, or agent rules, preserve them and document any divergence rather than overriding them.
- **Safe to Re-run:** Can be executed repeatedly without side effects. Existing valid configuration is recognized and left intact.

---

## 2. Deterministic Bootstrap Procedure

When `/factory-bootstrap` is invoked, execute the following 6-step workflow:

### Step 1: Detect Current Repository State
Inspect the repository environment:
1. Root structure and VCS state: `.git` root, current branch, working tree cleanliness.
2. Package and runtime manifests:
   - Node/TypeScript: `package.json`, `pnpm-lock.yaml`, `package-lock.json`, `tsconfig.json`
   - Python: `pyproject.toml`, `requirements.txt`, `Pipfile`, `poetry.lock`
   - Go: `go.mod`, `go.sum`
   - Rust: `Cargo.toml`, `Cargo.lock`
   - Java/Kotlin: `pom.xml`, `build.gradle`, `build.gradle.kts`
   - PHP: `composer.json`
   - Other languages and platforms.
3. Tooling configurations: test frameworks (Jest, Vitest, Pytest, Go test), build tools, linters, formatters.
4. Database & persistence: ORM configs, migration folders, schema files, connection templates (`.env.example`).
5. Deployment & containerization: `Dockerfile`, `docker-compose.yml`, CI/CD workflows (`.github/workflows/`).
6. Existing AI instructions and agents: `AGENTS.md`, `CLAUDE.md`, `.cursorrules`, `.agents/`, `.gemini/`.

*Security rule:* Never read or output actual secret values from `.env` files or environment variables.

### Step 2: Determine Project Classification
Classify the repository into one of four distinct states based on concrete evidence:
- **Type A — New / Empty Project:** Fresh repository or blank folder with no preexisting application source code.
- **Type B — Existing Software Project:** Preexisting software codebase with established source files and tooling, but without Universal AI Software Factory components.
- **Type C — Already Factory-Enabled Project:** Project containing Factory worker agents, rules, and orchestration docs that may need health validation, updating, or alignment.
- **Type D — Factory Repository Itself:** The core template repository (`universal-ai-software-factory`) containing baseline agents, documentation, and skills.

### Step 3: Inspect Existing Project Conventions
Locate and review existing project-level conventions:
- Documentation: `README.md`, `CONTRIBUTING.md`, architectural design records (`docs/adr/`, `docs/architecture/`).
- Code style: `.editorconfig`, ESLint, Prettier, Ruff, Black, Rustfmt configs.
- Environment & config templates: `.env.example`, `.env.sample`.
- Workflow rules: existing prompt or agent guidelines.
*Constraint:* Never overwrite or silently replace existing project conventions with Factory defaults.

### Step 4: Detect Conflicts & Divergences
Identify potential incompatibilities:
- Conflicting agent rules or multiple overlapping sources of truth (`CLAUDE.md` vs `AGENTS.md`).
- Existing custom subagents that overlap with the six Factory worker roles.
- Existing tool or MCP server configurations.
- Divergent Git or branching safety policies.
- Conflicting test runner or linting standards.
*Constraint:* Report detected conflicts explicitly in the assessment report rather than unilaterally altering them.

### Step 5: Produce Project Profile Artifact
Generate a concise durable profile document at:
`docs/factory/project-profile.md`

Include:
- Project Classification (Type A/B/C/D)
- Language, runtime, and framework versions
- Package manager and dependency management strategy
- Build, test, and lint/formatting toolchain
- Database & persistence layer (if detectable)
- Deployment, CI/CD, and containerization context
- Existing AI agent systems and customization directories
- Factory compatibility assessment
- Unresolved integration risks (e.g. missing test harnesses, ambiguous architecture)
*(Never include credentials or private paths).*

### Step 6: Produce Bootstrap Assessment Report
Generate the durable health and action report at:
`docs/factory/bootstrap-report.md`

Evaluate the project's overall status against four levels:
- `NOT_BOOTSTRAPPED`: No Factory components present.
- `PARTIALLY_BOOTSTRAPPED`: Some components exist (e.g., `AGENTS.md` present, but missing worker agents or `docs/orchestration.md`).
- `BOOTSTRAPPED`: All required Factory baseline components are present and valid.
- `NEEDS_REVIEW`: Conflicts or incompatible customizations detected requiring human architectural decisions.

The report must outline:
1. Overall Bootstrap Status
2. Detected Technology Stack & Project Conventions
3. Factory Baseline Component Checklist (Present vs. Missing)
4. Conflicts & Divergences Detected
5. Recommended Next Actions
6. Required Human Decisions (if any)

---

## 3. Factory Component Expectations

A fully bootstrapped Factory project contains the following components:

```text
├── AGENTS.md                                # Central repository governance & rules of engagement
├── docs/
│   ├── orchestration.md                     # Gated multi-agent lifecycle & verification rules
│   ├── integrations/                        # (Optional) Project-specific integration guides (e.g. mysql-mcp.md)
│   └── factory/                             # Durable profile and bootstrap assessment reports
│       ├── project-profile.md
│       └── bootstrap-report.md
└── .agents/
    ├── agents/                              # The six specialized Factory worker agents
    │   ├── researcher/agent.md
    │   ├── product-analyst/agent.md
    │   ├── architect/agent.md
    │   ├── developer/agent.md
    │   ├── reviewer/agent.md
    │   └── qa/agent.md
    └── skills/                              # Reusable workflows and skills (e.g., factory-bootstrap)
```

### Component Authority & Relationship
- **`AGENTS.md`**: Master authority for repository rules, Git safety, coding standards, and agent boundaries.
- **`docs/orchestration.md`**: Authoritative execution policy for phase gates (Discovery → Spec → Architecture → Implementation → Verification → Finish), remediation loops, and handoff artifacts.
- **`.agents/agents/*`**: Declarative subagent configurations defining role boundaries, permissions (`enable_mcp_tools: false`), and instructions.
- **`.agents/skills/*`**: Self-contained, task-specific procedural workflows.
- **`docs/factory/project-profile.md`**: Durable snapshot of detected project facts and runtime environment.
- **`docs/factory/bootstrap-report.md`**: Assessment outcome, status indicator, and recommended next steps.

---

## 4. Subagent Delegation During Assessment & Adoption

While the Main Agent orchestrates the bootstrap procedure, it should delegate specialized exploration to worker subagents when deep domain investigation is needed:
- **Unfamiliar or Complex Technology Stack:** Delegate deep exploration of obscure libraries, runtimes, or configurations to `researcher`.
- **Ambiguous Requirements / Domain Modeling:** Delegate analysis of business requirements, user journeys, or domain models in an existing codebase to `product-analyst`.
- **Architectural Assessment & Seam Identification:** Delegate mapping of modules, dependencies, and integration seams to `architect`.
- **Scaffolding or Code Adaptation (when explicitly approved by user):** Delegate code creation or adaptation to `developer`.
- **Code Standards & Security Audit:** Delegate static quality, vulnerability, and convention checks to `reviewer`.
- **Test Harness Verification:** Delegate running existing test suites and verifying test coverage health to `qa`.

---

## 5. Idempotency & Re-running

When invoked on an already Factory-enabled project (Type C or D):
1. Verify that all 6 worker subagents are present in `.agents/agents/` with valid YAML frontmatter and `enable_mcp_tools: false`.
2. Verify that `AGENTS.md` and `docs/orchestration.md` exist and match project standards.
3. Update `docs/factory/project-profile.md` and `docs/factory/bootstrap-report.md` with current repository facts.
4. If no discrepancies are found, report status `BOOTSTRAPPED` with no unnecessary file alterations.

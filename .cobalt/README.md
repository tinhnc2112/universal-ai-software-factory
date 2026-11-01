# Cobalt Operational Runbook

## 1. Purpose

This file is the operational runbook for running this repository inside Cobalt Agent Computer.

It defines:

* How to bootstrap the repository
* How to install dependencies
* How to identify the project runtime
* How to configure environment variables
* How to start development services
* How to identify application ports
* How to verify the application
* How to use Preview and Agent Browser
* How to handle databases and persistent local state
* How to recover from a failed or suspended environment

This file is operational documentation.

Coding conventions and agent behavior belong in:

```text
AGENTS.md
```

---

# 2. Repository

Repository:

```text
universal-ai-software-factory
```

Primary source of truth:

```text
GitHub repository
```

Expected local project root:

```text
C:\Users\tinhn\Desktop\universal-ai-software-factory
```

Do not treat the Cobalt Agent Computer filesystem as the permanent source of truth.

Important project state should be represented by:

* Source code
* Configuration files
* Documentation
* Database migrations
* Lockfiles
* Git commits

---

# 3. Current Project Type

This repository is currently a reusable AI software factory / agent configuration repository.

It is not currently a specific production application.

The repository primarily contains:

```text
.agents/
skills-lock.json
AGENTS.md
.cobalt/
```

Application-specific runtime instructions must be added when an actual software project is introduced.

Do not invent an application framework, package manager, database, port, or development command when one has not yet been established.

---

# 4. Required Files

The core factory configuration is expected to contain:

```text
AGENTS.md
.cobalt/
└── README.md

.agents/
└── skills/
    └── <skill>/
        └── SKILL.md

skills-lock.json
```

Additional project files may be introduced later.

---

# 5. First-Run Bootstrap

At the beginning of a new Cobalt Agent Computer session:

1. Read `AGENTS.md`.
2. Read this file.
3. Inspect repository structure.
4. Inspect Git status.
5. Detect the project type.
6. Detect the package manager.
7. Detect runtime requirements.
8. Detect database requirements.
9. Detect required environment variables.
10. Identify development and verification commands.
11. Start only the services required for the requested task.

Do not blindly execute commands copied from another project.

Verify that commands match the current repository before running them.

---

# 6. Environment Discovery

When a real application is added to this repository, determine the runtime from repository files.

Inspect files such as:

```text
package.json
pnpm-lock.yaml
package-lock.json
yarn.lock
bun.lock
pyproject.toml
requirements.txt
uv.lock
go.mod
Cargo.toml
Dockerfile
docker-compose.yml
compose.yml
.env.example
```

Also inspect:

```text
README.md
docs/
```

Use the repository's existing package manager whenever one is clearly established.

Do not introduce a second package manager without an explicit reason.

---

# 7. Dependency Installation

For JavaScript / TypeScript projects:

Prefer the package manager indicated by the existing lockfile.

Examples:

```text
pnpm-lock.yaml  -> pnpm
package-lock.json -> npm
yarn.lock -> yarn
bun.lock / bun.lockb -> bun
```

For Python projects:

Prefer the repository's established environment manager and dependency file.

Examples:

```text
pyproject.toml
uv.lock
requirements.txt
poetry.lock
```

For other ecosystems, follow the repository's native dependency management.

After installation:

* Check for installation errors.
* Confirm the expected dependency tree exists.
* Do not modify lockfiles unnecessarily.

---

# 8. Environment Variables

Never hard-code secrets into the repository.

Expected patterns include:

```text
.env
.env.local
.env.example
```

When environment configuration is required:

1. Inspect existing documentation.
2. Inspect `.env.example` or equivalent.
3. Determine which variables are required.
4. Configure secrets through the environment supported by Cobalt.
5. Keep secret values out of Git.
6. Document variable names and purpose, not secret values.

Example documentation format:

```text
DATABASE_URL=<configured through environment>
AI_API_KEY=<configured through environment>
AI_MODEL=<configured through environment>
```

Never commit actual credentials, API keys, access tokens, or passwords.

---

# 9. Services

For a real application, document each required development service.

Use a structure such as:

| Service  | Purpose          | Start Command |            Port |
| -------- | ---------------- | ------------- | --------------: |
| Web      | Main application | `<command>`   |        `<port>` |
| API      | Backend API      | `<command>`   |        `<port>` |
| Worker   | Background jobs  | `<command>`   | `<port or N/A>` |
| Database | Persistent data  | `<command>`   |        `<port>` |

Only list services that are actually required.

Do not start unnecessary services merely because they exist in configuration.

---

# 10. Port Management

Before starting an application:

1. Check whether the configured port is already in use.
2. Inspect existing development processes.
3. Reuse a healthy existing service when appropriate.
4. Avoid killing unrelated processes.

Record important application ports in this file once verified.

Example:

```text
Web Preview:
http://localhost:3000
```

Do not claim a port is available until it has been verified.

---

# 11. Development Startup

For a newly discovered application:

1. Identify the official development command from the repository.
2. Verify dependencies are installed.
3. Configure required environment variables.
4. Start the minimum required service set.
5. Confirm the process remains healthy.
6. Confirm the expected port is reachable.
7. Confirm the application loads before beginning browser verification.

Typical commands may include:

```text
npm run dev
pnpm dev
yarn dev
bun dev
python ...
docker compose up ...
```

These are examples only.

Never assume a command applies to this repository without checking the project configuration.

---

# 12. Preview

When the repository contains a browser-accessible application:

1. Start the application.
2. Confirm the preview endpoint is reachable.
3. Use the Cobalt Preview/Agent Browser environment where appropriate.
4. Verify the requested user flow against the acceptance criteria.

Do not treat a successfully started process as proof that the application is functionally correct.

---

# 13. Agent Browser Verification

Use Agent Browser when browser-level verification materially helps the task.

Typical workflow:

```text
Start application
       ↓
Confirm Preview
       ↓
Open application
       ↓
Navigate to affected page
       ↓
Perform required user actions
       ↓
Inspect visible result
       ↓
Inspect console/network when relevant
       ↓
Record result
```

Prioritize verification of the exact acceptance criteria.

Examples:

* Authentication flow
* Form submission
* CRUD operations
* Navigation
* Search
* Filtering
* File uploads
* Payment-related UI behavior
* Publishing workflows
* Error states
* Responsive UI behavior

Do not perform arbitrary browser exploration when it provides no value.

---

# 14. Verification Commands

Every real application should document its verified checks.

Typical categories:

### Type checking

```text
<verified command>
```

### Linting

```text
<verified command>
```

### Unit tests

```text
<verified command>
```

### Integration tests

```text
<verified command>
```

### Build

```text
<verified command>
```

### Browser verification

```text
<verified procedure>
```

Do not replace `<verified command>` with invented commands.

Once the real application establishes these commands, update this file with the exact working commands.

---

# 15. Verification Standard

Before declaring a significant task complete:

* Verify the requested functionality.
* Run relevant automated checks.
* Verify affected browser flows when applicable.
* Check for obvious runtime errors.
* Inspect relevant logs when something fails.
* Report failures honestly.

A passing build is not automatically proof that the feature works.

A passing typecheck is not automatically proof that the UI works.

A working browser page is not automatically proof that data persistence works.

Use the verification level appropriate to the change.

---

# 16. Database

When an application uses a database:

1. Identify the database engine.
2. Identify the connection configuration.
3. Identify migration tooling.
4. Identify seed data requirements.
5. Determine whether the database is local, containerized, remote, or managed.
6. Document the development setup.

Preferred documentation:

```text
Database:
Engine: <engine>
Host: <host>
Port: <port>
Database: <database>
Migration command: <command>
Seed command: <command>
Reset command: <command>
```

Do not run destructive database commands unless explicitly authorized or they are clearly documented as part of the task.

Examples of potentially destructive operations:

```text
database reset
drop database
drop table
truncate
destructive migration
delete seed data
```

Treat real or production-like data as protected.

---

# 17. Database Recovery

When the local database becomes unusable:

1. Determine whether the database contains disposable development data.
2. Check the project's documented recovery procedure.
3. Prefer migrations and reproducible seed scripts.
4. Avoid destructive resets unless authorized.
5. Document any recovery step that future agents will need.

If database state is important to reproducing the application, ensure the state can be recreated from repository-controlled configuration.

---

# 18. Persistent State

Cobalt Agent Computer may retain local runtime state such as:

* Dependencies
* Build outputs
* Local databases
* Containers
* Caches
* Volumes
* Generated application files
* Running processes

Do not assume this state is guaranteed forever.

Durable project state belongs in GitHub/repository-controlled files.

When a runtime dependency can be recreated automatically, prefer reproducible setup over relying on manually configured state.

---

# 19. Recovery After Suspension or Restart

After a Cobalt computer is resumed or recreated:

1. Read `AGENTS.md`.
2. Read this file.
3. Inspect Git status.
4. Confirm expected source files exist.
5. Confirm dependencies.
6. Confirm environment configuration.
7. Confirm database state.
8. Restart required services.
9. Confirm Preview.
10. Re-run the relevant verification.

Never assume previous running processes are still available.

---

# 20. Logs

When a service fails:

1. Inspect the relevant process output.
2. Identify the first meaningful error.
3. Determine whether it is:

   * dependency related
   * environment related
   * database related
   * port related
   * code related
   * network related
   * external-service related
4. Fix the root cause when within task scope.
5. Re-run the affected verification.

Do not repeatedly restart a failing service without understanding the failure.

---

# 21. External Services

Applications may depend on services such as:

* GitHub
* AI model APIs
* Payment providers
* Email providers
* Cloud storage
* Social media APIs
* MCP servers
* Webhooks

For each external service, document:

```text
Service:
Purpose:
Authentication:
Required environment variables:
Development setup:
Verification method:
Failure mode:
```

Do not place credentials in this file.

---

# 22. MCP

Cobalt supports configured remote MCP integrations.

When an MCP integration is required:

1. Confirm the integration is configured in Cobalt.
2. Confirm the required authentication is available.
3. Understand what actions the MCP server can perform.
4. Use the minimum required permissions.
5. Verify important external actions.

Do not assume local `stdio` MCP servers are available in Cobalt.

Prefer supported remote MCP integrations.

Never write MCP credentials into the repository.

---

# 23. Git Workflow

Before substantial implementation:

```text
git status
```

After implementation:

```text
git status
git diff
```

When appropriate, inspect:

```text
git diff --stat
git diff
```

Do not discard unrelated user changes.

Do not force-push unless explicitly requested.

Do not reset the repository merely to make the working tree clean.

Commit related changes together when the task calls for a commit.

Push important completed work to GitHub so the repository remains the durable source of truth.

---

# 24. Repository Hygiene

Do not commit generated or secret files unless the project explicitly requires them.

Common files that may belong in `.gitignore`:

```text
.env
.env.local
node_modules/
dist/
build/
.next/
coverage/
*.log
local database files
temporary files
```

Use the project's existing `.gitignore` conventions when available.

Do not blindly add patterns that could hide required source files.

---

# 25. Factory Repository Specifics

The current factory repository contains reusable agent skills.

The primary skill directory is:

```text
.agents/skills/
```

The installed skill set is managed by:

```text
skills-lock.json
```

Do not manually delete individual skills unless there is a clear reason.

When updating the skill collection:

1. Review the source.
2. Understand what will change.
3. Keep the lockfile synchronized.
4. Check for duplicate installations.
5. Check the resulting Git diff.

---

# 26. Adding a New Application to the Factory

When this factory is used as the foundation for a new software project:

1. Define the application requirements.
2. Establish the technology stack.
3. Establish the package manager.
4. Establish the database if required.
5. Add the application source tree.
6. Add reproducible setup instructions.
7. Configure environment variables.
8. Configure development services.
9. Configure verification commands.
10. Update this runbook.
11. Update `AGENTS.md` if the project's engineering rules change.

The runbook must describe the actual application rather than remaining generic.

---

# 27. Operational Decision Rules

When uncertain:

### Prefer repository evidence over memory

Inspect files and configuration.

### Prefer reproducible commands over manual procedures

A future Agent Computer should be able to reconstruct the environment.

### Prefer the smallest required service set

Do not start unnecessary infrastructure.

### Prefer reversible operations

Avoid destructive operations unless required and authorized.

### Prefer explicit documentation

Document important operational decisions.

### Prefer actual verification over assumptions

Never claim "working" without evidence.

---

# 28. Change Log

Record important operational changes here when they affect how future Cobalt tasks should run.

Format:

```text
YYYY-MM-DD — Change
Reason:
Operational impact:
```

Example:

```text
2026-10-07 — Initial Cobalt runbook created
Reason: Establish reusable operating procedure for Universal AI Software Factory
Operational impact: Future Cobalt tasks should read this file during startup
```

---

# 29. Current Bootstrap Status

At factory initialization:

```text
[✓] Git repository created
[✓] Matt Pocock skills installed
[✓] .agents/skills configured
[✓] skills-lock.json created
[✓] AGENTS.md configured
[✓] Cobalt operational runbook configured
[ ] Cobalt Workspace connected
[ ] GitHub Workspace integration configured
[ ] Muse Code configured
[ ] Agent Computer verified
[ ] Agent Browser verified
[ ] Remote MCP integrations configured
[ ] Worker Roles configured
```

Update this section as the factory configuration progresses.

---

# 30. Completion Rule

When an operational change is made to the repository:

1. Verify the change.
2. Update this runbook if the change affects future setup or operation.
3. Check Git diff.
4. Commit when appropriate.
5. Push durable changes to GitHub when appropriate.

The goal is that a new Cobalt Agent Computer can understand and reproduce the required project environment without relying on undocumented manual knowledge.

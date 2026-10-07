# MySQL MCP Integration

The Universal AI Software Factory treats MySQL as an **OPTIONAL PROJECT-LEVEL integration**, not a Factory-global dependency. The Factory must be reusable across projects, so it should never hardcode a specific application database into the repository.

## 1. Architecture

MySQL MCP should be configured on a per-project/workspace basis. 

**Global Factory Setup:**
* **GitHub MCP** → Main Agent
* **Browser** → Antigravity built-in
* **MySQL MCP** → NOT globally configured

**Project Workspace Setup:**
* **MySQL MCP** → Optional
* **Main Agent** → Only the Main Agent receives access by default (`enable_mcp_tools: true`)
* **Worker Agents** → Worker agents must remain disabled (`enable_mcp_tools: false`). If they need database context, they must request it from the Main Agent.

## 2. Security Model

Security for the MySQL MCP integration is handled through strict layers of defense. The order of trust is as follows:

1. **MySQL database privileges** are the *hard security boundary*. (Cryptographically enforced by the database engine).
2. **MCP server configuration** is the *capability boundary*. (Restricts the tool execution space).
3. **Antigravity agent MCP enable/disable** is the *agent access boundary*. (Determines which agents can invoke the tools).
4. **Agent instructions** are *behavioral guidance only*. (Soft instructions, susceptible to prompt injection or hallucination).

**Recommended Privileges:**
Use a dedicated MySQL account with only the minimum privileges required for the current project context. For a typical read-only development connection, the conceptual privileges are:
* `SELECT`
* `SHOW VIEW`

## 3. Example Project-Level Configuration

When a project requires database access, place the configuration in the workspace-level `.agents/mcp_config.json` file. 

Below is a clearly marked placeholder example using the explicitly read-only `@benborla29/mcp-server-mysql` implementation:

```json
{
  "mcpServers": {
    "mysql": {
      "command": "npx",
      "args": ["-y", "@benborla29/mcp-server-mysql"],
      "env": {
        "MYSQL_HOST": "127.0.0.1",
        "MYSQL_PORT": "3306",
        "MYSQL_USER": "<PROJECT_READONLY_USER>",
        "MYSQL_PASS": "<PROJECT_SECRET>",
        "MYSQL_DB": "<PROJECT_DATABASE>",
        "ALLOW_INSERT_OPERATION": "false",
        "ALLOW_UPDATE_OPERATION": "false",
        "ALLOW_DELETE_OPERATION": "false",
        "ALLOW_DDL_OPERATION": "false"
      }
    }
  }
}
```

## 4. Installation Policy

The MySQL MCP server should **only** be installed and configured when the current project actually requires MySQL inspection or query access. Do not install it in the Universal AI Software Factory repository by default.

## 5. Production Safety

To ensure maximum safety for both the Factory and the target environments, strictly adhere to the following rules:

* **Never connect the factory to a production database by default.**
* **Never use the MySQL `root` user for MCP.**
* **Never store credentials in `AGENTS.md`.**
* **Never store credentials in `.agents/agents/<name>/agent.md`.**
* **Never commit credentials to Git.**
* Prefer connecting to a dedicated development or test database.
* Prefer configuring the MCP server as read-only first.
* Enable write operations only for a specific, isolated project requirement and with explicit user approval.

## 6. Version Note

The current tested local environment for this architecture is **MySQL 8.0.46 on Windows**. However, the Factory is designed to be database-agnostic and does not depend on this exact version.

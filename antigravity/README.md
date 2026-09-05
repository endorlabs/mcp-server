# Endor Labs MCP Server

The Endor Labs Model Context Protocol (MCP) server brings software supply chain security into your AI-powered development workflow. It scans your code as you write, so you can catch issues long before they are a problem in production. Your AI agent gets real-time, proactive security insights and automated fixes in your editor, while you build.

The server runs locally on your machine as a lightweight process and communicates over stdio. When the AI agent needs security context, the server calls the Endor Labs cloud platform and returns the results.

## Features

An editor configured to use the Endor Labs MCP server can use its AI capabilities to help you:

- **Provide guardrails for agents before code review:** Reduce the number of known vulnerabilities entering your code and save developers time by checking AI agent suggestions in real time. Integrate security before an issue is discovered in CI or in production.
- **Improve the speed of remediating security risks:** Agents use vulnerability context from Endor Labs to help implement secure changes, from writing more secure code to upgrading dependencies.

## Prerequisites

- [Node.js](https://nodejs.org/) 18 or later (24 LTS recommended), with npm included.
- **Developer Edition** (free): no account and no configuration needed.
- **Enterprise Edition**: an Endor Labs namespace, Read-Only permissions or higher, and browser access for authentication (a browser window opens automatically on first use).

## Install & Configuration

1. In the Antigravity MCP Store, click the **Install** button.
2. Optional: add your Enterprise Edition inputs in the configuration pop-up, then click **Save**. Leave all inputs empty to use the free Developer Edition.

| Input | Description |
|:------|:------------|
| Endor Labs Namespace | (Optional) Your Endor Labs tenant namespace. Enterprise Edition only. |
| Authentication Mode | (Optional) Enterprise Edition authentication method: `github`, `gitlab`, `google`, or `sso`. |
| SSO Tenant | (Optional) SSO tenant name. Only needed when the authentication mode is `sso`. |

You can now see all enabled tools in the **Tools** tab.

### Usage

Once configured, the MCP server automatically provides Endor Labs security capabilities to your AI assistant. You can:

- **Check dependencies:**
  - "Check if the npm package lodash version 4.17.20 has any vulnerabilities"
  - "Is log4j-core 2.14.1 safe to use?"
- **Scan your project:**
  - "Scan this repository for vulnerable dependencies and leaked secrets"
- **Fix what is found:**
  - "Upgrade the vulnerable dependencies in this project to safe versions"

## Server Capabilities

The Endor Labs MCP server provides the following tools:

| Tool Name | Description |
|:----------|:------------|
| `check_dependency_for_vulnerabilities` | Check if a dependency in your project is vulnerable. |
| `check_dependency_for_risks` | Check a dependency for security risks including vulnerabilities and malware. |
| `get_endor_vulnerability` | Get the details of a specific vulnerability from the Endor Labs vulnerability database. |
| `get_resource` | Retrieve additional context from commonly used Endor Labs resources about your software, such as findings, vulnerabilities, and projects. |
| `scan` | Run an Endor Labs security scan to detect risks in your open source dependencies, find common security issues, and spot any credentials accidentally exposed in your Git repository. |
| `security_review` | Perform security review analysis on code diffs. Requires the Enterprise Edition, a namespace in the MCP server configuration, and [AI security code review](https://docs.endorlabs.com/secure-ai-coding/ai-security-review/) enabled for your namespace. |

After you set up the MCP server, you can choose to disable the tools that you do not want to use.

## Custom MCP Server Configuration

The Endor Labs MCP server is configured using environment variables. All of them are optional; the free Developer Edition runs with no configuration.

```bash
export ENDOR_NAMESPACE="<your-namespace>"                    # Optional: Enterprise Edition only
export ENDOR_MCP_SERVER_AUTH_MODE="<github|gitlab|google|sso>"  # Optional: Enterprise Edition only
export ENDOR_MCP_SERVER_AUTH_TENANT="<your-sso-tenant>"      # Optional: only with sso auth mode
```

Add the following configuration to your MCP client (e.g., `mcp_config.json` for Antigravity):

```json
{
  "mcpServers": {
    "endor-labs": {
      "command": "npx",
      "args": ["-y", "endorctl", "ai-tools", "mcp-server"]
    }
  }
}
```

The server uses the stdio transport only, so configure it with a `command` and `args` entry rather than an HTTP (`type: http`) URL.

## Documentation

- For more information, visit the [Endor Labs MCP documentation](https://docs.endorlabs.com/setup-deployment/mcp)

---
name: manage-codex-marketplace
description: Maintain this repository's Codex plugin marketplace. Use when adding or updating a plugin, bundled skill, marketplace entry, MCP server configuration, local installation/testing workflow, or ChatGPT public-submission materials in this repository.
---

# Manage Codex Marketplace

Maintain the repository as a repo-scoped, local Codex marketplace. Inspect the current tree before editing; the expected layout below may not exist yet.

## Project layout

```text
.
├── .agents/plugins/marketplace.json   # Curated repo marketplace catalog
├── .codex/skills/manage-codex-marketplace/ # This maintenance skill
└── plugins/
    └── <plugin-name>/
        ├── .codex-plugin/plugin.json  # Required plugin manifest
        ├── skills/<skill-name>/SKILL.md
        ├── .mcp.json                  # Optional bundled MCP configuration
        ├── .app.json                  # Optional registered ChatGPT MCP mapping
        ├── hooks/hooks.json            # Optional lifecycle hooks
        └── assets/                     # Optional install-surface assets
```

Keep plugin and skill names in lowercase kebab-case. Keep manifest and marketplace paths relative, `./`-prefixed, and inside their respective roots.

## Select the change

| Request | Edit | Read first |
| --- | --- | --- |
| Add or change a reusable workflow | `plugins/<plugin>/skills/<skill>/SKILL.md` | [Build skills](https://developers.openai.com/plugins/build/skills) |
| Add or change plugin metadata/components | `plugins/<plugin>/.codex-plugin/plugin.json` | [Package your plugin](https://developers.openai.com/plugins/build/plugins) |
| Expose, order, or set policy for a plugin | `.agents/plugins/marketplace.json` | [Marketplace metadata](https://developers.openai.com/plugins/build/plugins#marketplace-metadata) |
| Add/change a bundled MCP server | `plugins/<plugin>/.mcp.json` plus its implementation | [Build an MCP server](https://developers.openai.com/plugins/build/mcp-server) |
| Connect a registered ChatGPT developer-mode MCP server | `plugins/<plugin>/.app.json` and `apps` in the manifest | [Package and test an MCP-backed plugin](https://developers.openai.com/plugins/build/plugins#create-and-test-a-plugin-locally-with-an-mcp-server) |
| Test a complete plugin | Marketplace, then the relevant plugin | [Connect and test](https://developers.openai.com/plugins/deploy/connect-chatgpt) |
| Publish publicly | Submission materials and deployed MCP server, if applicable | [Submit plugins](https://developers.openai.com/plugins/deploy/submission) and [Plugin guidelines](https://developers.openai.com/plugins/app-guidelines) |
| Resolve installation, validation, or submission failures | Affected file/configuration | [Submission error reference](https://developers.openai.com/plugins/deploy/submission-errors) and [Plugin troubleshooting](https://developers.openai.com/plugins/guides/troubleshooting) |

Use the linked official documentation as the source of truth for fields, compatibility, and current platform steps. Browse it when a request relies on a current product behavior or schema.

## Local marketplace workflow

1. Put each plugin in `plugins/<plugin-name>/` with `.codex-plugin/plugin.json`.
2. Add exactly one matching entry under `plugins[]` in `.agents/plugins/marketplace.json`; use `source: { source: "local", path: "./plugins/<plugin-name>" }`.
3. Include `policy.installation`, `policy.authentication`, and `category` for every marketplace entry.
4. Check configured sources with `codex plugin marketplace list`. Register this repository explicitly when needed with `codex plugin marketplace add .`.
5. For desktop-app testing, restart ChatGPT, select the marketplace in Plugins Directory, install the plugin, and test in a new conversation. Local installs use a cached copy, so repeat the refresh/install cycle after source changes.

Do not edit the user's global Codex or ChatGPT configuration unless the request explicitly asks for a personal marketplace or global plugin settings.

## Plugin and skill editing

For a skills-only plugin, keep `plugin.json` minimal: `name`, `version`, `description`, and `skills: "./skills/"`. Add install-surface metadata only when needed.

Make skill descriptions specific enough to communicate when the workflow applies. Keep task instructions in `SKILL.md`; create bundled scripts, references, or assets only when the workflow needs them. Validate the plugin paths and JSON after editing.

For bundled MCP, ensure tool names, descriptions, schemas, and annotations accurately match behavior. Mark tools that write, publish, send, or delete as non-read-only and ensure confirmation/approval behavior is appropriate. Test tools directly before testing model selection.

## Publishing boundaries

Treat local marketplace distribution, workspace sharing, and public-directory submission as distinct:

- Local marketplace: authoring, private testing, and repo/CLI distribution.
- Workspace sharing: share a created plugin with selected workspace users; it is not public publication.
- Public directory: submit through the OpenAI Platform. Prepare verified publisher identity, listing and legal URLs, starter prompts, test cases, release notes, and policy attestations. For MCP plugins, deploy a reviewable public HTTPS endpoint and complete the required tool metadata and domain verification.

Before proposing public submission, check the [submission requirements](https://developers.openai.com/plugins/deploy/submission) and [plugin guidelines](https://developers.openai.com/plugins/app-guidelines). Do not claim approval or publish on the user's behalf without explicit authorization.

## Troubleshooting order

1. Confirm the marketplace file and plugin manifest exist, parse as JSON, and use the expected relative paths.
2. Run `codex plugin marketplace list` and verify the marketplace root and plugin source resolve as expected.
3. Refresh/restart the relevant host; local installs use cached copies.
4. For MCP issues, test reachability, transport, authentication, tool schemas, annotations, and direct tool calls before investigating model behavior.
5. For public submission failures, use the exact portal error with the [submission error reference](https://developers.openai.com/plugins/deploy/submission-errors); re-check the guidelines before re-submitting.

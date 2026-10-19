# Contributing

This repo holds the public connection guide, MCP registry metadata and the Claude Code plugin (skills, agents, evals) for Gillby. The server itself is hosted at https://www.gillby.com/mcp.

- **Bad data or a tool bug**: open an issue with the tool, spot and `meta.requestId`.
- **Skill or agent changes**: edit `skills/` or `agents/`, then run `claude plugin validate . --strict` and `claude plugin eval . --judge-model sonnet`. Include the eval summary in the PR.
- **New eval cases**: add a folder under `evals/` with a `case.yaml`. Use mocks under `evals/mocks/gillby/`; never call personal account tools in evals.

Gillby is not a navigation chart. Skills and agents must keep source freshness, advisory times and unavailable states in their answers and must never invent vessel details, departure times or readings.

# Evaluate trace quality

A simple skill for auditing agent traces in Braintrust to assess whether they look good and contain useful data for debugging and evaluation.

## Requirements

- Authenticated Braintrust MCP or `bt` CLI access to the target project.
- Access to official Braintrust docs through MCP docs search, the website, or current CLI-fetched docs.
- A specific trace URL/identifier (if analyzing a single trace), or a project and time window to sample.
- Optional but recommended: read access to the codebase for the agent / where tracing is implemented

## What it reviews

Trace boundaries and structure, span types and names, actual inputs/outputs, useful metadata, attachments, and substantial duplication or noise. It checks relevant Braintrust docs before making product-specific recommendations and uses source code, when available, to distinguish instrumentation defects from agent design issues.

Inspection is read-only. Findings cite trace evidence, provide an inferred severity, and explain the fix and verification

## Getting started

1. Copy `SKILL.md` and `references/` into your coding agents skills directory: `.claude/skills` for Claude Code; `.codex/skills` for Codex.
2. Provision access to Braintrust for your coding agent, either via the [Braintrust MCP](https://www.braintrust.dev/docs/integrations/developer-tools/mcp) or the `bt` [CLI](https://www.braintrust.dev/docs/reference/cli/quickstart). Make sure you are authenticated first.
   1. The CLI comes with the ability to query Braintrust docs directly. The MCP comes with MCP resources for agent specific documentation.
   2. You should also provide access to Braintrust documentation, which can be done either via the Docs MCP (go to Docs, hit the dropdown on "Copy Page" and select MCP) or if the coding agent has native URL scraping tools, it should be able to fetch docs from that method.
3. Optional but highly recommended - provide access to your codebase during the audit so the agent can ground it's analysis in the actual instrumentation.
4. Invoke the skill with a bit of context, including project name, number of traces to inspect, details about the agent, and anything useful info that is relevant for understanding the codebase and instrumentation.,
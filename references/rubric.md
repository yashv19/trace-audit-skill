# Trace quality review guide

Judge traces for the agent's use case. Check Braintrust docs before recommending changes. Cite trace evidence for every finding. Good traces don't need recommendations.

## Structure — the main check

Start here. The trace should clearly map what the agent actually did.

Each trace covers a conversation, turn, or clear agent task, with a clear start and outcome. Turn-level traces share a session ID in metadata.

For conversational agents, each turn clearly shows what the user said and how the agent replied, in order. Messages should parse into a back-and-forth thread view. A readable blob of conversation text is not enough. Repeated history should not appear as new turns.

Spans reflect execution. Tools and subagents sit under their callers. Parallel subagents aren't nested inside each other. Errors and retries are visible.

Use code, when available, to check the execution path. An accurate trace of a confusing agent flow points to an agent issue. Don't claim full coverage without evidence.

## Types and names

LLM calls are LLM spans; tool calls are tool spans. Confirm type mappings in the integration or OTEL docs.

Names describe the step: `search_documents` beats `step`. Flag confusing names, not style preferences.

## Span data

Log each step's actual input and output: messages and responses for LLMs, arguments and results for tools. Don't copy the user's question onto every span.

Metadata helps explain or filter runs: session, route, environment, release, or model. Require only useful fields.

Inspect retrieval results, tool results, and errors. Check timing, usage, and scores where relevant and supported. Don't require scores that were never calculated.

Before flagging missing data, rule out preview limits and deliberate redaction.

## Duplication and noise

Repeated conversation history is fine. Turn 4 may have turns 1–3 as input to a span. That's okay because that might be whats actually being passed to the LLM call.

Flag duplication or unrelated logs only when they obscure execution or greatly increase trace size. A few extra fields or spans are fine. Keep service data that explains agent behavior.

Things that are not fine are things like application logic that was invoked that is not relevant to the actual agent quality. For example, a bunch of proxy services that run before the agents called. That's not relevant to agent quality and introduces noise. 

## Attachments

Log files as Braintrust attachments. Check inputs, outputs, metadata, and nested fields for inline base64, byte arrays, or file blobs. Prompt text and useful document excerpts can stay inline.

Read the attachment docs first. Check automatic conversion and stored data before recommending manual uploads. Look for duplicate file bodies elsewhere.

For large spans, find the largest fields and check current limits. Check code or export errors for failed uploads; stored traces won't show them.

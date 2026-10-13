# Usage and performance report

Generated 2026-10-13T04:02:31.089Z from evidence/agent-usage.csv, runtime\telemetry\mcp-usage.jsonl.

Observed usage is what you read from your Copilot usage view before and after a work item, in GitHub AI Credits (current billing) or premium requests (legacy annual plans). It is not estimated: an empty value stays "not observed". AI Credits are computed from input, output and cached tokens at the model's price, so the context you send (instructions, attachments and tool results such as the MCP estimates below) is the part you control. MCP token figures use the ceil(characters / 4) heuristic and are estimates, not tokenizer counts.

## Agent work items

| Lab | Pass | Agent | Model | Prompts | Observed usage | Peak context % | Accepted | Rework cycles |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| lab2 | Build pass | pm-tool-engineer | GPT-6 Luna (copilot) | 1 | not observed | n/a | yes | 0 |

Accepted work items: 1. Prompts per accepted work item: 1.00. Rework cycles: 0.
No observed usage recorded; compare prompts, rework and context instead.

| Model | Prompts | Observed usage |
| --- | --- | --- |
| GPT-6 Luna (copilot) | 1 | not observed |

## MCP tool results entering model context

No tool calls recorded in runtime\telemetry\mcp-usage.jsonl.

## HTTP performance

Run with --metrics while the server is running to include live route percentiles.

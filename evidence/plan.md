# Approved increment plan

## Work item and contract version
Lab 2: bounded MCP release context and usage accounting. Contract version 1.0.

## Current SHA and known dirty changes
HEAD: `2e0e7c171abfb158833c621a2c9e1e66270620c3`. Pre-existing worktree changes: modified `evidence/readiness.md`; untracked `.github/agents/pm-coordinator.agent.md`, `.github/agents/pm-implementer.agent.md`, `.github/agents/pm-planner.agent.md`, `.github/agents/pm-reviewer.agent.md` and `.github/agents/pm-tool-engineer.agent.md`. Preserve these changes; they are outside this increment.

## Branch name and target pull request
Branch: `lab1/agent-governance`. Target: repository default branch (`main`). Pull request: not recorded yet.

## User outcome and exclusions
Provide bounded, current release context as advisory MCP read data, and make MCP tool result size and usage telemetry measurable while protecting product notes and argument values. No UI or HTTP API changes. No writes, arbitrary paths, shell tool, publishing or release authorization. Preserve the protocol transport and existing supplied tests/data.

## Allowed files and owner
- `mcp/tools.mjs`: implement only `LAB02_CONTEXT` for `get_release_context`.
- `mcp/usage.mjs`: implement only `LAB02_USAGE`, `LAB02_BUDGET` and `LAB02_RECORD`.
- `ci/accepted-suites.json`: append the Lab 2 focused suites `tests/lab02.test.mjs` and `tests/usage.test.mjs`; never remove existing suites.
- `evidence/plan.md`, `evidence/readiness.md` and `evidence/agent-usage.csv`: record the approved scope and actual evidence/usage.
- `tests/lab02.test.mjs` and `tests/usage.test.mjs` are read-only acceptance references. Do not modify them. `mcp/server.mjs` and data files are also out of scope.

## Implementation sequence
1. Implement `get_release_context` for R1/R2 using current state. Return only matching release features plus `contractVersion`, current `revision`, a `source` value and `advisory: true`. Reject invalid releases and preserve unknown-tool/extra-argument behavior.
2. Implement `estimateTokens` as `ceil(text.length / 4)`.
3. Implement `budgetResult`: return in-budget values unchanged; for over-budget values with an `items` or `features` array, truncate the list and report `truncated`, `omitted` and a useful hint while remaining within budget; otherwise fail closed with `RESULT_TOO_LARGE`.
4. Implement `usageRecord` with ISO timestamp, tool, sorted `argKeys`, UTF-8 `resultBytes`, estimated tokens, duration rounded to two decimals, `isError` and `truncated`. Do not include argument values or result text.
5. Register the focused Lab 2 suites in the accepted-suite list, run focused validation, capture actual output tied to the candidate SHA, and complete evidence/usage records.

## API and UI acceptance
MCP `get_release_context` retains its existing `{release}` schema (`R1` or `R2`) and returns only matching feature records with contract version, revision, source provenance and advisory labeling. MCP tool results keep the current server wrapper. No UI or HTTP endpoint changes. Usage results stay within the configured estimated-token budget or fail closed; telemetry remains one privacy-safe record per MCP call. Telemetry failures must not fail tool calls, and protocol messages remain the only stdout output.

## Checks and failure scenarios
- Focused check: `npm run lab:02`.
- Verify the newly accepted suites through `npm run ci:accepted` and the PR-required `accepted-checks`, `performance-budget` and `governance-guard` checks.
- Verify invalid release/arguments and unknown tools fail closed; data is not mutated; truncation preserves list metadata and reports the omitted count; oversized non-list results yield `RESULT_TOO_LARGE`.
- Verify telemetry contains only the specified fields and never feature notes, argument values or result text; telemetry write failures do not break tool calls.
- Live-client observation remains required for release-context results and protocol behavior. No commands have been run for this plan.

## Suites to add to ci/accepted-suites.json
Append `tests/lab02.test.mjs` and `tests/usage.test.mjs`. Keep `tests/baseline.test.mjs` and all previously accepted suites.

## Agent and model per step, expected prompts
Planning: `pm-planner`, model recorded in its profile as `GPT-6 Luna (copilot)`, using `prompts/01-plan.md` and the Lab 2 assignment in `prompts/02-tool.md`. Implementation: `pm-tool-engineer`, profile model `GPT-6 Luna (copilot)`, using `prompts/02-tool.md`. Record actual model, billing unit, prompts and observed usage in `evidence/agent-usage.csv`; the expected model is not a substitute for observed usage.

## Rollback and preserved evidence
If implementation fails, revert only the Lab 2 changes in a reviewed follow-up; do not weaken the accepted-suite list or alter tests/data/transport. Preserve the current dirty `evidence/readiness.md` and untracked agent profiles. Keep command output, candidate SHA, MCP observations and usage records for review.

## Product decisions
1. Define the `source` field value and provenance: `callTool` receives state but no source path, while the server may load either seed data or persisted runtime state. Do not invent misleading provenance.
2. Resolve the minimum-valid-budget behavior. `tests/usage.test.mjs` expects `budgetResult({items: []}, 10)` to throw `INVALID_INPUT`, but the prompt and contract do not define a minimum. Preserve the supplied test; do not edit it to settle the discrepancy.

## Human approval name and time
Accepted by Muhammad Fikri Abdul Jalil on 2026-10-13. Time not recorded.

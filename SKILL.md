---
name: p9-dev-doc-summarizer
description: Use when engineering documentation needs structured summarization from one or more files, especially when outputs must separate facts from inference, ask clarifying scope questions first, include optional diagrams, and publish to local files or a knowledge base.
---

# P9 Dev Doc Summarizer

## Overview

Produce a high-signal engineering summary from selected files without modifying source files. Extract context, architecture intent, change logic, key code paths, risk points, and verification suggestions, then optionally generate stable flow diagrams and publish to local or a knowledge base.

## Workflow

Follow this workflow in order.

1. Confirm input scope
- Ask which files or directories to summarize.
- Ask expected output language and audience (engineering, product, mixed).
- Ask destination: local, Yuque, or other knowledge base.
- Ask diagram mode: `mermaid` or `draw`.

2. Read-only ingestion
- Read target files only; do not edit, rewrite, or format source files.
- Prefer fast discovery first (`rg --files`, `rg`).
- For large repos, prioritize files by change relevance: entry points, service logic, data model, and interface contracts.

3. P9-level synthesis
- Build summary sections in this order:
  - Background and objective
  - Current pain point or trigger
  - Change strategy and trade-offs
  - Code path and key modules
  - Data flow and side effects
  - Risk and rollback notes
  - Verification and observability
- Tie every claim to concrete evidence from files.
- Separate facts from inferences. Mark inferences explicitly.

4. Diagram decision and generation
- Generate a diagram only when it improves clarity (cross-module logic, non-trivial control flow, async/event chains, or state transitions).
- Keep diagram deltas minimal across iterations:
  - Keep stable node IDs (e.g., `N1`, `N2`, `N3`) and only append new IDs.
  - Keep edge labels and direction style consistent.
  - Keep layout direction fixed (`flowchart LR` or `TB`) unless readability breaks.
  - Reuse previous node text when semantics do not change.
- Use templates in [references/diagram-templates.md](references/diagram-templates.md), [references/mermaid-template-library.md](references/mermaid-template-library.md), and [references/drawio-template-library.md](references/drawio-template-library.md).

5. Publish output
- Local mode:
  - Ask for output path and filename.
  - Write a Markdown summary with optional diagram block.
- Yuque mode:
  - Ask for space/repo, document title, and update/create preference.
  - Ask for required credentials or command path available in environment.
  - If publishing is blocked, generate a Yuque-ready Markdown payload locally and report exact next command.
- Other knowledge base mode:
  - Ask for target system and required publish format.
  - Generate a destination-ready payload first; publish only if tool/credential is confirmed.
- Use format guidance in [references/publish-templates.md](references/publish-templates.md).

## Platform Adapters

Keep core workflow platform-agnostic. Apply only the minimum adapter for runtime environment.

- Codex skill runtime:
  - `SKILL.md` is core behavior.
  - `agents/openai.yaml` is UI metadata only and can be ignored by non-Codex runtimes.
- CC runtime:
  - Reuse the same workflow as plain instruction text.
  - Use [references/cc-integration-template.md](references/cc-integration-template.md) as the drop-in template.

## Subagent Policy

Use subagents only when complexity justifies coordination cost.

- Default: no subagent.
- Enable subagents when at least one condition is true:
  - More than 8 files or more than 3000 lines of relevant content.
  - Need parallel extraction across independent modules.
  - Need separate perspectives (logic extraction vs diagram stabilization).
- Recommended split:
  - Subagent A: evidence extraction and factual summary.
  - Subagent B: diagram modeling with minimal-diff constraints.
  - Main agent: merge, de-duplicate, and final quality gate.

## Output Contract

When producing final output, use this section order.

1. Executive Summary
2. Background
3. Change Logic
4. Code Impact (files/modules/functions)
5. Risks and Mitigations
6. Validation Plan
7. Diagram (optional, `mermaid` or `draw`)
8. Destination Metadata (local path or knowledge-base target)

## Guardrails

- Never modify source files during summarization.
- Never invent code behavior not supported by evidence.
- Prefer concise but complete technical language.
- If evidence is missing, state what is missing and what to inspect next.

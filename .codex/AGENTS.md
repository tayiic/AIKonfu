# Codex Standard Harness

This `AGENTS.md` is the single AI entry file for agents working on or with `codex_standard/`.
Human rationale and onboarding live in [README.md](README.md).

## Single AI Entry

If an AI agent needs to learn, clone, or bootstrap this standard into a new project, it MUST start from this file.

- Do not start from `README.md`.
- Do not treat `ai_coding_standard/` as the execution entry for Codex bootstrap.
- Use `ai_coding_standard/` only as concept-layer reference when the method needs clarification.
- After entering here, follow the Truth Source Order below.

## Mission

Keep `codex_standard/` usable as a production-grade bootstrap layer for complex projects.
Prefer deterministic generation, minimal drift, and shareable output over clever one-off edits.

## Truth Source Order

When updating this harness, use this precedence:

1. [MANIFEST.json](MANIFEST.json)
2. [templates/project-preset.schema.json](templates/project-preset.schema.json)
3. [templates/](templates/)
4. [generators/generate-project.ps1](generators/generate-project.ps1)
5. [README.md](README.md)

Do not patch the README and leave the generator inconsistent.
Do not patch the generator and leave the schema or manifest stale.

## Operating Rules

- Treat `README.md` as human-facing and this file as the only AI-facing bootstrap entry.
- Keep generated project config machine-shareable.
- When applying this standard to an existing project, preserve project-specific behavior, naming, scripts, workflows, ports, and architecture unless the repository itself explicitly asks to replace them.
- Do not overwrite user-global Codex config under `~/.codex/` as part of normal project bootstrap or learning. Only touch global config when the user explicitly asks, or when the repository already declares that a specific global-facing file must be synchronized.
- Never generate absolute-path trust entries into project `.codex/config.toml`.
- Do not pin project-level `model`, `model_provider`, or `model_reasoning_effort` unless the project explicitly requires an override; prefer inheriting the user's global Codex defaults.
- Never generate secrets, tokens, or passwords into repository files.
- Keep runtime artifacts under `.codex/runs/` and out of version control.
- Use nested `AGENTS.md` files only where subtree-specific instructions add real value.

## Profiles

Profiles are real generation inputs, not labels:

- `minimal`: smallest safe set
- `standard`: default team delivery set
- `full`: enterprise-grade set with cross-model review capability

When editing profile behavior, update all of:

1. `MANIFEST.json`
2. `templates/project-preset.schema.json`
3. `generators/generate-project.ps1`
4. `README.md` if behavior changed

## MCP Rules

When you touch MCP generation:

- keep project output limited to shareable keys
- prefer explicit per-server approvals
- only mark `supports_parallel_tool_calls = true` when tool concurrency is actually safe
- preserve MCP initialization and capability-negotiation assumptions from the spec

## Required Verification

For changes to this harness, run the smoke test and verify:

1. no unresolved `{{...}}` placeholders remain in generated files
2. profile-specific asset selection is correct
3. generated `.gitignore` includes the managed runtime block
4. generated `.codex/config.toml` stays portable across machines

## Editing Style

- Keep the structure opinionated but compact.
- Prefer one clear mechanism over duplicated fallback mechanisms.
- If a helper is only for humans, put it in `README.md`.
- If guidance is only for AI execution, put it here or in generated `AGENTS.md` templates.

## v3.7 Additions

The following were added in v3.7 (sourced from GStack research):

### New Agents

| Agent | Profile | Purpose |
|-------|---------|---------|
| `product-challenger` | standard / full | Questions premises before implementation |
| `design-reviewer` | standard / full | UI/UX quality audit with 7-dimension scoring |

### New Workflows

| Workflow | Profile | Purpose |
|----------|---------|---------|
| `autoplan` | standard / full | Auto-chained review pipeline (ProductChallenge → Spec → Architecture → DesignReview → CodeReview → DXReview) |
| `context-handoff` | all | Session context save/restore for cross-session continuity |

### New Protocols

| Protocol | Profile | Purpose |
|----------|---------|---------|
| `product_challenge_protocol` | standard / full | Six forcing questions before coding |
| `design_review_protocol` | standard / full | 7-dimension design quality audit |
| `browser_qa_protocol` | full | Browser-based visual QA for web projects |
| `context_handoff_protocol` | all | Session state save/restore format |

### FSM-10-Stage Changes

The workflow is now FSM 12-Stage with three new interleaved stages:

- Stage 1.5: ProductChallenge (between Triage and Spec)
- Stage 3.5: DesignReview (between Architecture and Implement)
- Stage 6.5: BrowserQA (between Verify and Security)

All new stages have skip conditions and do not affect non-applicable projects.

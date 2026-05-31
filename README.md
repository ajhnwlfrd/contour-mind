# Contour Mind

Most AI-assisted development tools focus on code generation. They help you build once you already know what to build. Contour Mind focuses on the step before that — the step most people skip.

This is spec-driven development: the idea that the quality of what you build is determined before a single line of code is written.

## The problem

A coding agent can only execute well when the task is clear, bounded, and testable. But most software ideas don't start that way. They start as vague intentions, inherited assumptions, and half-formed requirements. When those go straight to implementation, you get code that solves the wrong problem well.

Contour Mind exists to close that gap.

## What it does

Contour Mind is a thinking layer — a guided discovery conversation that runs before coding. It helps you turn a rough idea into a structured, implementation-ready spec by asking the right question at the right time.

You don't need to know any frameworks. The system applies mental models invisibly in the background — abstraction laddering, issue trees, first principles, inversion, second-order thinking — and routes between them based on the shape of the conversation. You experience it as a natural, focused dialogue.

The output is two things:
- A structured markdown spec covering the problem statement, goals, non-goals, user flow, edge cases, failure modes, constraints, and MVP scope.
- A scoped implementation prompt ready to hand to a coding agent.

## How it works

The core is an adaptive router. Rather than walking you through a fixed questionnaire, it diagnoses what kind of thinking is needed next:

- If the idea is vague, it clarifies the level of abstraction.
- If the problem is large, it decomposes it into branches.
- If assumptions are weak, it tests them.
- If failure modes matter, it inverts the problem.
- If too many things compete, it helps you prioritize.

Once the spec is shaped, a second skill — `contour-verify` — checks it for completeness, consistency, and implementation readiness before anything gets built.

A spec only passes when implementation-critical ambiguity has been resolved or explicitly captured as an open decision with a default recommendation. Nothing vague gets through.

## The workflow

1. Run `contour-mind` — turn a rough idea into a structured spec.
2. Run `contour-verify` — check the spec for gaps before implementation.
3. Hand the final spec to your coding agent.

## Get started

This repo contains two ready-to-use skill setups — one for Claude Code, one for Codex.

- **Claude Code** → [claude/](claude/)
- **Codex** → [codex/](codex/)

Each folder has its own README with setup instructions.

**No full skill setup needed?** Copy [references/router-spec.md](references/router-spec.md) as `CLAUDE.md` or `AGENTS.md` into any project root. The agent will follow the full router logic immediately, without any additional configuration.

## Go deeper

- [docs/paper.md](docs/paper.md) — the full design paper: the router, the mental models, the three-layer architecture, and future work.
- [references/router-spec.md](references/router-spec.md) — the detailed router spec with routing tables, readiness checks, and closure gate.
- [examples/](examples/) — sample prompts showing the kind of ideas Contour Mind is built to handle.

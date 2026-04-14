<div align="center">
  <h1>🏭 CLI-Factory</h1>
  <strong>The missing standard for agent-reliable CLIs.</strong>
</div>

---

## The Problem

AI agents interact with the world through CLI commands. There are already great tools that produce CLIs — [bb-browser](https://github.com/anthropics/bb-browser) turns websites into commands, [CLI-Anything](https://github.com/HKUDS/CLI-Anything) generates harnesses for desktop apps, and more are emerging every week.

They all produce CLIs. **None of them produce agent-*reliable* CLIs.**

```bash
bb-browser site xiaoyunque/generate-video "prompt" "ref" "thread" "model" "720p"
# Returns a run_id. Then what?
#
# ❌ Is the job done?         → No lifecycle tracking
# ❌ How much did it cost?    → No budget awareness
# ❌ Where's the video?       → No artifact retrieval
# ❌ Timed out, retry?        → No idempotency
# ❌ Platform changed?        → No drift detection
# ❌ Structured error?        → No error classification
```

An agent calling this is flying blind.

## Why Now

The world is rapidly filling with generated CLIs. Browsers become commands, desktop apps become harnesses, internal tools get wrapped overnight.

That changes the bottleneck.

We no longer just need *more* CLIs. We need CLIs that agents can trust under real execution: long-running jobs, flaky platforms, budget constraints, retries, artifact collection, and operational drift.

Without a shared standard, every agent integration has to rediscover the same answers:

- What shape does success return in?
- How do I tell queued from running from finished?
- Can I retry safely?
- What did this run cost?
- Where are the outputs?
- Is the platform broken, or did my input fail?

CLI-Factory exists to make those answers consistent.

## What is CLI-Factory

**A Protocol** that defines what an agent-reliable CLI must provide — structured JSON output, job lifecycle, budget control, error taxonomy, idempotency, and drift detection.

**A Toolkit** that helps developers build new CLIs — or upgrade existing ones — to conform to that protocol.

```
  Existing CLI / Harness    Protocol Spec       Conformance Tests
           │                     │                       │
           └─────────────┬───────┴───────────────┬───────┘
                         ▼                       ▼
  ┌────────────────────────────────────────────────────────────┐
  │ CLI-Factory                                                │
  │                                                            │
  │ burns 🔥🔥🔥 ad-hoc behavior into explicit agent contracts │
  │ structured JSON • lifecycle • budget • retries            │
  │ artifacts • typed errors • drift detection                │
  └────────────────────────────────────┬───────────────────────┘
                                       ▼
                              Agent-Reliable CLI
```

## Example JSON Contract

Instead of making agents scrape terminal prose, a CLI-Factory command should return a predictable machine-readable shape.

```json
{
  "ok": true,
  "command": "video.generate",
  "request_id": "req_abc123",
  "status": "succeeded",
  "lifecycle": {
    "queued_at": "2026-04-14T10:00:00Z",
    "started_at": "2026-04-14T10:00:03Z",
    "finished_at": "2026-04-14T10:00:41Z"
  },
  "cost": {
    "estimated": 30,
    "actual": 28,
    "currency": "credits"
  },
  "artifacts": [
    {
      "kind": "video",
      "path": "./output/cat-piano.mp4"
    }
  ],
  "error": null
}
```

The exact schema will evolve, but the contract should always be explicit about outcome, lifecycle, cost, artifacts, and error state.

## Core Guarantees

CLI-Factory is opinionated about what an agent should be able to depend on:

- Structured output: every command returns machine-readable JSON, not prose that must be guessed at.
- Lifecycle visibility: async work exposes clear states like queued, running, succeeded, failed, and timed out.
- Budget awareness: estimation and execution can be guarded by explicit spend limits.
- Idempotent execution: retries should not accidentally duplicate costly work.
- Typed errors: agents can distinguish user error, platform failure, transient retryable issues, and drift.
- Artifact retrieval: outputs are discoverable, downloadable, and explicitly reported.
- Drift detection: when a target platform changes, the CLI can surface that clearly instead of silently degrading.

## Non-Goals

CLI-Factory is not:

- a replacement for tools like bb-browser or CLI-Anything
- an agent runtime, planner, or orchestration framework
- a guarantee that the underlying platform will be stable

Here's what a CLI-Factory-produced CLI looks like in practice:

```bash
# 1. Is the platform still working?
cli-factory-xiaoyunque doctor
# → {"ok": true, "status": "healthy", "checks": [...]}

# 2. How much will it cost?
cli-factory-xiaoyunque video estimate \
  --model seedance_2.0_fast --duration-sec 5
# → {"ok": true, "estimated_cost": 30, "remaining": 270, ...}

# 3. Generate with budget guard and idempotency
cli-factory-xiaoyunque video generate \
  --prompt "A cat playing piano" \
  --model seedance_2.0_fast \
  --duration-sec 5 \
  --output ./output/ \
  --max-credits 50 \
  --request-id "req_abc123"
# → Estimate → submit → poll → detect artifact → download → report cost
# → All in structured JSON. Every error typed. Every credit tracked.
```

Whether you have an existing CLI or are starting from scratch, CLI-Factory gives you the standard and the tools to make it agent-native.

## Roadmap

We're building CLI-Factory in layers:

1. Define the protocol: command contracts, lifecycle semantics, error taxonomy, and budget model.
2. Ship a reference implementation: one end-to-end CLI that demonstrates the protocol in practice.
3. Build the toolkit: scaffolding, adapters, and reusable components for upgrading existing CLIs.
4. Add verification: conformance tests so developers can prove their CLI is agent-reliable.
5. Grow backend coverage: support more web, desktop, and internal-tool integrations over time.

## Current Status

> **Early stage — defining the protocol.**
>
> We're writing the Agent Execution Protocol spec and building the first reference implementation. The vision is clear; the code is being written.

## Join Us

We're looking for developers who believe CLIs should be first-class citizens in the agent ecosystem.

If you're interested in protocol design, CLI development, or integrating new backends — [open an issue](https://github.com/fernwen1114-collab/CLI-Factory/issues) or [start a discussion](https://github.com/fernwen1114-collab/CLI-Factory/discussions).

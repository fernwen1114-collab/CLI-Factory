<div align="center">
  <h1>🏭 CLI-Factory</h1>
  <strong>The standard + factory for agent-native CLIs.</strong>
  <br/><br/>
  English | <a href="README_zh.md">中文</a>
</div>

---

## The Problem

AI agents interact with the world through CLI commands. There are already great tools that produce CLIs — turning websites into commands, generating harnesses for desktop apps, wrapping internal tools. More are emerging every week.

These tools solve the access layer. CLI-Factory solves the reliability layer — with a shared standard.

Take a real example. bb-browser can call a video generation platform:

```bash
bb-browser site xiaoyunque/generate-video "prompt" "ref" "thread" "model" "720p"
```

The access works. But for an agent, the hard questions start after the call returns:

- ❌ Is the job done? — No lifecycle tracking
- ❌ How much did it cost? — No budget awareness
- ❌ Where's the output? — No artifact retrieval
- ❌ Timed out, retry? — No idempotency
- ❌ Platform changed? — No drift detection
- ❌ Structured error? — No error classification

This is the reliability gap. CLI-Factory fills it.

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

**A Spec** that defines what an agent-native CLI must provide — structured JSON output, job lifecycle, budget control, error taxonomy, idempotency, and drift detection.

**A Toolkit** that helps developers build new CLIs — or upgrade existing ones — to conform to that protocol.

```
  Your Config              Spec              Test Suite
       │                    │                     │
       ▼                    ▼                     ▼
  ┌──────────────────────────────────────────────────┐
  │               CLI-Factory  🔥                    │
  │          burns tokens to vibe-code your CLI      │
  │                                                  │
  │          any CLI-producing backend               │
  └────────────────────────┬─────────────────────────┘
                           │
                           ▼
                   Agent-Native CLI
```

## Core Guarantees

CLI-Factory is opinionated about what an agent should be able to depend on:

- **Structured output**: every command returns machine-readable JSON, not prose that must be guessed at.
- **Lifecycle visibility**: async work exposes clear states — queued, running, succeeded, failed, timed out.
- **Budget awareness**: estimation and execution can be guarded by explicit spend limits.
- **Idempotent execution**: retries should not accidentally duplicate costly work.
- **Typed errors**: agents can distinguish user error, platform failure, transient retryable issues, and drift.
- **Artifact retrieval**: outputs are discoverable, downloadable, and explicitly reported.
- **Drift detection**: when a target platform changes, the CLI surfaces that clearly instead of silently degrading.

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
> We're writing the spec and building the first reference implementation. The vision is clear; the code is being written.

## Join Us

We're looking for developers who believe CLIs should be first-class citizens in the agent ecosystem.

If you're interested in protocol design, CLI development, or integrating new backends — [open an issue](https://github.com/fernwen1114-collab/CLI-Factory/issues) or [start a discussion](https://github.com/fernwen1114-collab/CLI-Factory/discussions).

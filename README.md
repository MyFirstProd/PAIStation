# PAIStation

**A local-first operating system for professional AI agents, computer control, voice, and versioned digital selves.**

> **Status:** public concept and architecture draft. The implementation is still private while the security boundary, provider integrations, and data model are being validated.

PAIStation is an attempt to build one dependable personal agent that can:

- work on real software projects with the discipline of modern coding agents;
- use files, terminals, browsers, and desktop applications;
- perceive the physical world through cameras and microphones, and control a safe robotic body;
- communicate naturally by text and voice;
- switch between cloud and local models without locking the user into one provider;
- preserve private, dated snapshots of a person's memories, language, values, and decision style.

This is not a chatbot with hundreds of loosely connected tools. It is a supervised agent runtime with explicit permissions, verifiable execution, durable memory, and replaceable intelligence backends.

## Why this project

Today's strongest assistants are split across separate products. Coding agents understand repositories but are not a persistent personal presence. Voice assistants are easy to talk to but weak at long-running work. Computer-use agents can click through interfaces but are difficult to trust. Memory features usually flatten a changing person into one continuously overwritten profile.

PAIStation brings these capabilities under one user-owned control plane.

The long-term experiment is inspired by the Ship of Theseus: instead of creating one synthetic personality that constantly rewrites itself, PAIStation preserves immutable snapshots. A future user should be able to speak with a transparent reconstruction of their 2026, 2036, or 2056 self without later beliefs silently replacing earlier ones.

## Product modes

### Operator

A professional work agent for repositories, research, documents, browser workflows, and desktop tasks. Every run has a goal, scope, permission budget, success criteria, and evidence of completion.

### Companion

A private conversational presence with its own identity and relationship history. It can know the owner deeply without pretending to be the owner.

### Time Capsules

Dated reconstructions of the owner, built from source-backed memories, recordings, writing, decisions, and explicit self-descriptions. A Time Capsule always discloses that it is a reconstruction and never invents unsupported biography.

These modes may share infrastructure, but they never share identity implicitly.

### Embodied Agent

A physical presence that can see, hear, orient itself, and eventually move through the world. The private prototype already includes a USB camera, microphone, an Arduino-controlled two-axis pan/tilt mount, and face-tracking software. The same contracts are designed to grow toward depth sensors, a mobile base, manipulators, and other robot hardware without coupling the agent to one device.

## Architecture

```mermaid
flowchart TD
    UI[Desktop / Web / Voice] --> SESSION[Session & Task Manager]
    SESSION --> MODE[Identity and Privacy Mode]
    MODE --> POLICY[Permission Broker]
    POLICY --> ORCH[Agent Orchestrator]

    ORCH --> CODEX[Codex Backend]
    ORCH --> CLAUDE[Claude Backend]
    ORCH --> LOCAL[Local Model Backend]
    ORCH --> TOOLS[Typed Tool Runtime]

    SENSORS[Camera / Microphone / Future Sensors] --> PERCEPTION[Perception Runtime]
    PERCEPTION --> ORCH

    TOOLS --> FS[Files / Shell / Git]
    TOOLS --> WEB[Browser / MCP / APIs]
    TOOLS --> PC[Desktop Computer Use]
    TOOLS --> HWSAFE[Hardware Safety Runtime]
    HWSAFE --> BODY[Pan/Tilt / Future Robot]

    ORCH --> VERIFY[Result Verification]
    VERIFY --> EVENTS[Event Log & Artifacts]
    EVENTS --> MEMORY[Memory Curator]
    MEMORY --> SNAPSHOTS[Versioned Personal Snapshots]
```

### The important boundary

Models propose actions. Deterministic code validates and executes them.

No model receives direct, unrestricted access to the machine. The permission broker decides whether an action is read-only, reversible, external, or destructive. Untrusted content from websites, email, documents, and screenshots is treated as data and cannot grant itself more authority.

## Codex, Claude, and local models

PAIStation does not try to reproduce frontier agent intelligence inside a small local model. Instead, it provides a stable control plane and connects specialized backends through official interfaces.

### Codex adapter

- Rich product integration through the [Codex App Server](https://learn.chatgpt.com/docs/app-server).
- Programmatic coding tasks through the [Codex SDK](https://learn.chatgpt.com/docs/codex-sdk).
- Reuses Codex concepts such as threads, streamed events, approvals, sandboxing, and resumable work.

### Claude adapter

- Local or self-hosted agent execution through the official Claude Agent SDK.
- Optional hosted sessions through Claude Managed Agents.
- Direct Messages API mode for applications that need to own the tool loop.

### Local adapter

- OpenAI-compatible endpoints such as LM Studio, vLLM, or llama.cpp servers.
- Local inference for sensitive memory, offline conversations, embeddings, routing, and low-cost tasks.
- Model loading and VRAM budgeting are handled by a dedicated model manager.

Provider integrations must use supported authentication flows. PAIStation will not proxy, scrape, or bypass consumer product access. Users connect their own accounts, API credentials, or local runtimes where required.

## Model selection without provider chaos

The default interface should offer three choices, not a wall of model IDs:

1. **Auto** — route by task, privacy, required tools, latency, quality, and configured budget.
2. **Local only** — never send task content outside the machine.
3. **Choose backend** — pin a specific Codex, Claude, OpenAI API, or local backend to the current thread.

An advanced panel can expose individual models and reasoning settings.

Every backend publishes a capability manifest:

```yaml
id: codex-local
kind: agent
capabilities: [coding, shell, git, vision, long_running]
privacy: cloud
supports:
  streaming: true
  resume: true
  approvals: true
  sandbox: true
limits:
  context: provider_managed
  parallel_runs: configurable
```

Before a task starts, the UI should make five things visible:

- selected backend and model;
- local or cloud processing;
- tools and data sources being granted;
- expected approval behavior;
- fallback order if the preferred backend fails.

The user can pin a backend per thread. Automatic fallback must never cross a privacy boundary silently.

## Computer use

PAIStation follows a reliability hierarchy:

1. structured API or MCP integration;
2. filesystem, shell, or application CLI;
3. browser DOM and accessibility APIs;
4. screenshot-driven mouse and keyboard control as the final fallback.

GUI control is powerful but probabilistic. Sensitive actions require previews and explicit confirmation. On Windows, foreground computer use must also make it obvious when the agent owns the keyboard and pointer, and offer an immediate stop control.

## Embodied intelligence

PAIStation treats cameras, microphones, motors, and future robot hardware as first-class capabilities rather than ad-hoc tools.

Perception is split into two speeds:

- **Fast perception** runs continuously with conventional vision and signal processing for tracking, motion, safety zones, and low-latency reactions.
- **Semantic perception** sends selected frames or events to a vision-language model when the agent needs to understand objects, scenes, text, people, or a developing situation.

Continuous video is not continuously streamed into an LLM. A deterministic perception runtime decides when a meaningful event or representative frame should enter model context.

The embodiment layer is defined by three stable interfaces:

- `PerceptionProvider` produces timestamped observations with provenance and confidence;
- `ActuatorProvider` exposes semantic actions such as orient, follow, stop, move, or grasp;
- `SafetyController` enforces physical limits independently of model output.

The current pan/tilt station is therefore the first robot head, not a disposable demo. Future devices can be added as adapters. ROS 2 can become a bridge when the hardware grows complex enough, without making it a dependency of the initial desktop agent.

Physical safety remains below the model: calibrated limits, acceleration caps, watchdogs, collision and workspace constraints, manual takeover, and an emergency stop cannot be overridden by a prompt.

## Voice

Voice is an interface to the same task runtime, not a separate assistant.

The first production voice loop is deliberately simple:

`push-to-talk / VAD -> ASR -> task runtime -> streamed text -> TTS`

Natural interruption, echo cancellation, full duplex, and voice cloning come later. Transcripts and audio have independent retention controls. A cloned voice is never evidence that a generated statement was actually spoken by the person.

## Versioned digital selves

Continuous self-training is not the foundation. Raw life data is append-only, generated answers never become ground truth, and each snapshot has a cutoff date.

A snapshot contains:

- source-backed episodic and semantic memories;
- values, preferences, beliefs, and uncertainty;
- writing and speech style references;
- important decisions and the reasoning behind them;
- a voice model reference, when consent and data quality allow it;
- model, prompt, retrieval index, adapter, and evaluation versions;
- provenance and confidence for every claim;
- access, inheritance, deletion, and export policy.

Fine-tuning or lightweight adapters may improve style later, but identity remains portable outside model weights.

## Planned stack

| Layer | Planned technology |
|---|---|
| Agent runtime | Python 3.11+, asyncio, Pydantic |
| Local API | FastAPI, WebSocket / JSON-RPC |
| Desktop shell | Tauri 2, React, TypeScript |
| Coding backend | Codex App Server and Codex SDK |
| Claude backend | Claude Agent SDK / Messages API / Managed Agents |
| Local inference | LM Studio and other OpenAI-compatible runtimes |
| Browser | Playwright, isolated browser profiles |
| Windows automation | UI Automation first, visual computer use as fallback |
| Voice | VAD, faster-whisper or Qwen ASR, pluggable TTS |
| Perception | OpenCV fast path, event-triggered vision-language models |
| Embodiment | Typed sensor/actuator contracts, Arduino serial today, optional ROS 2 bridge later |
| Memory | SQLite, full-text search, sqlite-vec, immutable source archive |
| Security | OS sandboxing, capability grants, secret redaction, audit log |
| Observability | Structured events, traces, replayable task runs |
| Quality | pytest, scenario evals, browser replay, hardware-in-the-loop where applicable |

The stack is intentionally modular. Provider models, speech engines, vector search, and UI surfaces can change without rewriting the permission system or personal archive.

## Production principles

- Local-first does not mean local-only.
- Structured tools are preferred over visual clicking.
- Fast deterministic perception is preferred over sending every camera frame to a model.
- Every consequential action is attributable and reviewable.
- Verification is part of execution, not an optional final step.
- Personal memories need provenance, confidence, consent, and deletion controls.
- A companion and a reconstruction of the owner are different identities.
- No autonomous learning from the agent's own unreviewed output.
- Physical safety constraints live below the model and cannot be prompt-overridden.
- Exportability matters: a digital legacy must survive any single model provider.

## Initial roadmap

### 0. Public architecture and snapshot protocol

Publish design decisions, threat model, provider contracts, and the first personal-snapshot schema. Begin collecting a private baseline snapshot while the software is still being built.

### 1. One production vertical slice

Complete a repository task end to end: understand the request, inspect files, edit safely, run verification, present a diff, and resume after interruption.

### 2. Backend adapters and model picker

Connect Codex, Claude, and one local OpenAI-compatible runtime behind a shared task contract. Add explicit privacy boundaries and fallback rules.

### 3. Browser, desktop, voice, and embodiment

Add computer use incrementally, beginning with structured browser and accessibility control. Add a push-to-talk voice surface to the same sessions. Connect the existing camera, microphone, and pan/tilt prototype through stable perception, actuator, and safety contracts.

### 4. Time Capsule v1

Create the first immutable, source-backed personality snapshot and a regression suite for biographical fidelity, values, style, uncertainty, and non-fabrication.

## What is public today

For now, this repository contains the idea and architecture only. Source code will be published selectively after the core permission model and private-data boundaries are ready for external review.

If this direction resonates with you, star the repository and open a discussion describing the one workflow you would trust a personal agent to handle every day.

## Independence

PAIStation is an independent project and is not affiliated with or endorsed by OpenAI or Anthropic. Product and company names are used only to describe optional integrations.

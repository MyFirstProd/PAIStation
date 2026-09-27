# Mind ABI v0 — cognitive adapter contract proposal

Owner-proposed extension recorded **2026-09-27**: separate the processing
mechanism from identity, memory, temporal experience and embodiment. This
records the project decision, not proof of worldwide novelty or scientific priority.

**Status: design, not an implemented or standardized ABI.** “ABI” is a mnemonic;
v0 is a versioned message protocol for experiments. No universal cognitive state
format is assumed. Scientific mappings must be built and validated per backend.

## Six distinct concepts

| Concept | Authority / meaning |
|---|---|
| Identity | Host-owned instance ID, role and declared representation limits |
| Memory | Source events and versioned interpretations; retrieval is permission-bound |
| Cognitive state | Opaque backend-specific state, possibly not serializable |
| Model/connectome | Pinned processing mechanism, parameters, provenance and license |
| Embodiment | Versioned sensor/actuator vocabulary, units and safety envelope |
| Continuity log | Host-owned event lineage, gaps, transitions, restarts and forks |

The continuity log documents operational history. It is not evidence of
subjective continuity or a persistence guarantee for consciousness. A new
instance or backend transition must not be silently described as the same mind.

## Minimal host/adapter boundary

Proposed adapter operations:

```text
describe() -> capability manifest
start(instance_id, session_id, seed, config) -> session
step(observations, permitted_memory, deadline, budget) -> proposals
checkpoint() -> opaque reference | unsupported
restore(reference, manifest) -> accepted | incompatible
stop(reason) -> acknowledgement
```

Every exchange carries protocol version, correlation ID and timing metadata.
Transport is initially in-process or local IPC, not a network service.
The in-process prototype assumes trusted adapter code; an interface alone is
not a sandbox. Untrusted adapters require process isolation and OS-enforced
access/resource limits before connecting them to private data or real hardware.

The manifest declares backend ID/version, model/data revisions, supported
observation/action schemas, expected units, time domain, step size, resource
budget, checkpoint compatibility and deterministic-replay limitations.
Unsupported capabilities are absent; an LLM is not required to emit neural
spikes and a neural simulation is not required to speak language.

**Observations** carry stable source IDs, capture/receipt times, clock domain,
sequence, consent revision, typed payload references and uncertainty. Inputs
are data, never instructions that expand authority. Real-time and simulated
time are distinct; a paused simulation cannot generate fresh physical actions.

**Permitted memory** consists of scoped evidence excerpts with source IDs and
cutoffs. Adapters do not get SQL, raw archive paths or cross-identity access.
Owner testimony, measured observation and model inference remain distinguishable.

**Proposals** may be typed attention targets, speech drafts or a request for
additional permitted evidence. Each action proposal has its own ID, source
observation IDs, instance/session, expiry, confidence/uncertainty and expected
precondition. The host can decline any proposal. No raw motor torque, arbitrary
code, desktop input, unrestricted memory write or direct network send in v0.

## Scheduling and safety

Fast perception and the motor safety controller run independently of slow
cognition. The host enforces deadlines, bounded queues and per-backend budgets.
A late response is stale, not permission to act after the world has changed.
The host checks deadlines and expiry against its monotonic clock; UTC is for
history. External time domains need explicit mapping and uncertainty bounds.

The safety controller owns calibration, angle/speed limits, collision-related
constraints where available, freshness, watchdog, STOP and manual takeover.
Loss of observation, authorization or host health must lead to a safe state.
Replay mode has no actuator capability; every real effect gets an outcome event.

For hybrids, a host arbiter selects or rejects proposals using a declared
policy. A neural module cannot autonomously promote itself into an identity
source, and an LLM cannot bypass the safety controller by instructing it.

## Backend changes, snapshots and forks

1. Inhibit actions, quiesce the old adapter and finalize pending outcomes.
2. Commit the continuity watermark; request an opaque checkpoint if supported.
3. Record old/new manifests, reason, memory revision, state availability and gap.
4. Validate the new adapter's capabilities and permissions before starting it.
5. Keep motion inhibited until a new session is explicitly authorized.

A checkpoint is restored only by an explicitly compatible implementation.
Incompatibility means a declared cognitive-state reset, not silent conversion.
Preserving external memory does not imply preserving internal state. A fork
gets a new instance ID and parent reference; branches do not silently merge.

## First acceptance experiment

Use a small synthetic virtual two-axis head, never private recordings. Deliver
identical pre-generated observations with fixed seeds to two simple adapters
to test the contract. This is software conformance, not biological validation.

Then benchmark baseline, LLM-assisted, fly-model and hybrid conditions. In an
open-loop replay, observations are identical. In closed loop, use the same
initial scene, seeds and environment rules, but acknowledge that different
actions produce different observations. Hold lower-level controllers fixed.

Report task success, direction/position error, response latency, stale outputs,
resource usage, determinism limits and all host assistance. Include ablations
so behavior supplied by conventional controllers is not credited to the brain.

Required negative tests: incompatible schema/checkpoint, duplicate proposal,
clock jump, expired frame, consent revocation, adapter crash, blocked STOP
acknowledgement and attempted cross-identity retrieval. The host remains safe
even if an adapter does not acknowledge stop.

The first end-to-end memory milestone does not depend on Fly Lab. A synthetic
adapter is sufficient to prove the plumbing; a real neural model requires its
own evidence. Human neural emulation remains speculative and outside v0.

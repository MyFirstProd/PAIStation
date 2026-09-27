# Embodied Memory v1 roadmap

Decision: 2026-09-27. **Proposed gates, not completed results.**

Computer control is parked without deleting its implementation. The project
now prioritizes durable embodied experience over more application integrations.

## M0 — Scope and contracts

Create an explicit embodied-memory runtime profile without desktop, browser or
shell capabilities. Preserve legacy profiles for deliberate manual use.

Define a versioned observation envelope: stable event ID, session, source ID and
sequence, capture time, receipt time, monotonic clock domain, timing uncertainty,
consent revision, payload reference and provenance. Derived records list their
source IDs. Distinguish observations, owner testimony, model hypotheses,
confirmed facts, commands, outcomes and gaps.

Old records with receipt-time-only metadata remain labeled as such; never
fabricate capture timestamps. Migrations are additive and preceded by a tested
backup. Implement the minimal [Mind ABI](MIND_ABI.md) host/adapter contract using
synthetic data before connecting a neural simulation.

**Gate:** replay duplicates, reordering, clock jumps, source withdrawal and
restart. Stable IDs and provenance survive; missing intervals are explicit;
no repeated physical actions or computer-control capabilities are available.

## M1 — Reliable body and capture

Decouple capture from model latency. Bound queues, apply backpressure, record
heartbeat and gap events, forecast disk use, and stop visibly on quota failures.

Shutdown first inhibits motion and issues STOP, then halts intake, drains queues
within a deadline, and commits a durable checkpoint with any loss/unknown window
reported. Restart preserves confirmed events, never replays actuator commands,
and does not silently resume physical movement.

An event is acknowledged as stored only after a durable archive transaction
commits. Intake or queue admission is not a durable acknowledgement; an
uncommitted tail after failure is reported as an unknown/gap interval.
Voice can request stop, but ASR latency/failure cannot replace independent
UI/hardware emergency stop controls.

Revalidate actual wiring, calibration, stale-frame rejection, serial recovery,
watchdog, emergency stop and manual takeover. A **static-camera** memory run may
proceed independently; it must not be reported as passed motor acceptance.

**Gate:** one-hour fault-injection/replay plus separate supervised hardware
checks. Every acknowledged event survives restart. Simulated disk-full,
disconnect and process failure produce visible degraded state, not false health.

## M2 — The first actual 24 hours

Use fast conventional perception continuously and invoke speech/vision/language
models on selected events. Save episodes with intervals, source references and
uncertainty. Run idempotent overnight or idle consolidation; summaries are
derived evidence, not new ground truth.

Next-day recall must cite recorded events and abstain when evidence is absent.
Thoughts and motives are owner-reported statements, not deductions from video.

Before starting, choose an explicit finite raw-media retention policy and disk
budget. Mono PCM at 16 kHz/16 bit alone uses about 2.76 GB/day before overhead;
video adds more. Do not promise month-long recording from a small archive quota.

**Gate:** a real 24-hour report with coverage, gaps, latency, queue and disk
metrics. Preserve 100% of acknowledged synthetic marker IDs without duplicates.
On a prewritten evaluation: at least 18/20 correct source-backed recall answers,
and 10/10 abstentions for deliberately unsupported questions. Explain every
interruption. Short tests and simulated time do not satisfy the wall-clock gate.

## M3 — 30 days and recoverable continuity

Maintain temporal facts and explicit conflicts: an updated belief does not
overwrite the earlier belief's historical validity. Propagate corrections,
exclusions and deletion to retrieval and derived summaries.
If withdrawn data entered training, quarantine affected adapters until a
reviewed replacement/retraining excludes it. Track training-source dependencies;
do not promise reliable instantaneous unlearning from model weights.

Build encrypted export with user-controlled recovery material, integrity
verification and an actual restore in a clean test environment. An OS-profile-
bound encryption copy is not portable recovery. Use a consistent database
backup method rather than copying a live database without its pending state.

Replace the base LLM, exercise planned restarts and repeat held-out recall
checks. A model change and an internal-state reset are separate recorded events.

**Gate:** 30 calendar days with no silent history reset, visible gaps, restore
evidence and the same recall/privacy evaluations before and after migration.
Snapshots retain their cutoff and provenance across model changes.

## M4 — Personalization after evidence

First establish a retrieval-plus-authored-profile baseline. Curate separately
consented examples of situation, perception, owner self-report, response,
owner-stated rationale and outcome. Separate train/eval by time; exclude secrets
and ineligible third-party data. A month of capture is not automatic training
readiness or permission.

An identity snapshot manifest includes cutoff, source revisions, policy,
model/prompt/index/adapter versions, evaluations and representation limits.
Small adapter experiments require measured memory capacity, comparison to the
baseline and rollback. Model weights are not the authoritative memory store.

**Gate:** reproducible improvement without degrading biographical fidelity,
uncertainty or data boundaries. Do not train on unreviewed model-generated
biography. No claim of copied consciousness follows from style imitation.

## Operational controls

Separate switches: camera, microphone, telemetry, raw retention, tracking,
speech, consolidation, training and export. Display active sources and gaps.
Observation pause preserves existing records; STOP inhibits movement even if
the model or archive is unhealthy. Save-and-exit reports a checkpoint or failure.
Persist source preferences, but fail closed on uncertain motion authorization.

Use existing local hardware first. Capture/storage/light perception should not
depend on a large model being loaded. Schedule GPU work under a shared budget
and measure p95 latency, memory pressure and archive coverage under contention.
Choose the smallest model that passes the task-specific evaluation before
considering another large download. A separate inference node is optional later.

## Research lane and publication

Fly Lab gets at most two working days or 10% of effort before M2, whichever
budget is exhausted first. It starts with synthetic inputs and a virtual body.
No measurable benefit over a simple baseline: preserve the negative result and
park it. The core must remain usable with Fly Lab absent.

Publish documentation, schemas, synthetic fixtures and reproducible evaluations
only after review. Personal sensor streams and identity data stay private.
Actual source publication needs its own privacy/history and license decision.

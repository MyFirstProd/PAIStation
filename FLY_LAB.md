# Fly Lab — a bounded research lane

Research checked: 2026-09-27. **No fly backend has been integrated or benchmarked
on this project's hardware.** The plan below is our engineering hypothesis.

## Evidence, not an upload claim

| Resource | What it provides | What it does not establish |
|---|---|---|
| [FlyWire / Dorkenwald et al.](https://www.nature.com/articles/s41586-024-07558-y) | Adult-female fly connectome: 139,255 neurons and roughly 50 million chemical synapses | Connectivity alone does not specify every dynamic parameter, learning rule or biological state |
| [Shiu et al. model](https://github.com/philshiu/Drosophila_brain_model) | Executable leaky-integrate-and-fire model; sensorimotor predictions studied for feeding/grooming | Not a general mind or a ready-made camera controller |
| [Flyvis](https://github.com/TuragaLab/flyvis) and its [paper](https://www.nature.com/articles/s41586-024-07939-3) | Connectome-constrained visual model and pretrained experiments, including optic-flow tasks | Not owner recognition, autobiographical memory or a whole organism |
| [NeuroMechFly / FlyGym](https://github.com/NeLy-EPFL/flygym) | Embodied sensory and physical simulation | A body simulator is not itself a brain reconstruction |
| [Eon's March 2026 integration](https://eon.systems/updates/embodied-brain-emulation) | A closed neural-model/body/sensory loop | Authors report hand-chosen mappings, strong dependence on existing body controllers, largely missing plasticity and unvalidated internal dynamics |

These projects make useful experiments possible. None of the cited results
establishes consciousness transfer, full biological equivalence or a route to
copying a particular human from recordings. Eon's use of “upload” is the
authors' interpretation, not a premise of PAIStation.

## First experiment: visual attention

1. Pin versions, licenses and resource limits in an isolated research environment.
2. Generate synthetic moving dots/bars and fixed held-out scenes. No owner video.
3. Run a conventional optical-flow controller as the baseline.
4. Compare a Flyvis-based adapter, or an explicitly labeled fly-inspired model,
   on the same observations, seeds, timing and resource budget.
5. Emit an `attention_target` proposal with confidence, timestamp and source ID.
   Start in a two-axis **virtual** camera environment; record resulting views.
6. Only after simulation and separate physical safety acceptance may an adapter
   propose goals to the real safety controller. It never writes serial commands.

Predeclare gates: at least 90% direction accuracy on held-out clean stimuli;
report noisy/lighting-shift results separately; p95 processing at most 100 ms
on the actual machine. Set and record RAM/VRAM/queue limits before a run. The
baseline and experimental backend must not degrade core capture coverage.
Numbers are proposed targets, **not measured results**.

Compare tracking error, overshoot, stability, latency and energy/resource use.
If the neural adapter adds no useful capability within two working days / the
allocated 10% budget, archive the result and return effort to Embodied Memory.

## Later experiment: hybrid cognition

Through [Mind ABI](MIND_ABI.md), a fast neural controller could propose
orientation while an LLM proposes semantic goals and speech. The host owns
arbitration; neither backend can broaden the other's permissions or overwrite
observations with generated descriptions. Compare baseline-only, neural-only,
LLM-assisted and hybrid conditions; hold the environment and lower-level
controller fixed and log all assistance. This distinguishes the contribution
of the neural model from that of its engineered body/controller.

Checkpoint support is backend-specific. Sharing an observation/action protocol
does not make fly neural state convertible into LLM state or human identity.

## Compute and licensing constraints

The [Shiu code](https://github.com/philshiu/Drosophila_brain_model) documents a
Brian2/C++ route; the [Eon implementation](https://github.com/eonsystemspbc/fly-brain)
offers GPU backends. Neither is yet measured on this project's shared workstation.
Do not infer real-time performance from a dataset download size or a demo video.

Code and data have separate terms. Flyvis and the Shiu repository identify MIT
code licenses; FlyGym identifies Apache-2.0; Eon's repository identifies
GPL-2.0-or-later. Verify the exact pinned revision and weights/assets before use.
FlyWire public-release data is separately offered under
[CC BY-NC 4.0](https://flywire.ai/guidelines). A permissive code license does not
remove those data conditions. Do not bundle connectomes, weights or derived
data into the core distribution without a separate terms review.

Windows compatibility must be tested for the chosen pinned stack. An isolated
WSL environment is an option, not a requirement to alter the application's
working environment. Do not train a whole-brain controller or download large
datasets until the smaller benchmark justifies doing so.

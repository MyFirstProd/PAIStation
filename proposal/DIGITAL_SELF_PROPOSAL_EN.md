# PersonalAIStation — "The Digital Self"

**A research project on personal continuity: a body, a memory, time-sliced selves, and a legacy for descendants**

**Author and founder:** Alexander D. Budanov

**Contacts:** it@alexbudanov.ru · Telegram @alexandr_budanov_it

**GitHub (public concept and this document):** https://github.com/MyFirstProd/PAIStation

**Author's channel (projects, hardware, work in progress):** https://t.me/error404_engineer_not_found

**Document date:** October 6, 2026 · Version 1.0

**Status:** working prototype running at the founder's home; research program described below

---

## 1. The idea in one paragraph

We are building a local system that lives beside a person for years: it sees
through a motorized camera, listens, remembers only what the person has
consented to, stores every event with its source and time, and from that corpus
forms **dated slices of a personality** — like growth rings in a tree. The 2026
slice and the 2036 slice are two different "selves" that can be compared,
restored and questioned. The horizon goal is not a chatbot "in Alexander's
style" but an honest, verifiable copy of memory, manner and values that a person
leaves to children and grandchildren: something they can talk to, ask for
advice, and hear in the original voice. We call this the **Theseus experiment**:
if you replace one plank a year, at what point does the ship stop being the same
ship — and can that be measured?

## 2. Why now

- Local language models now run on a consumer GPU with quality sufficient for
  live dialogue. Personal data no longer has to leave the house.
- The first complete connectome of an adult fruit-fly brain has been published
  (FlyWire, 2024: 139,255 neurons, ~50 million synapses) along with executable
  models built on it. This opens a real rather than fantastical path: first
  learn to move sensorimotor decisions onto a scanned fly brain, then onto more
  complex organisms, and be ready for the moment human brain scans exist.
- People already leave terabytes of photos and messages behind, yet none of it
  can **answer**. The gap between "archive" and "interlocutor" is empty. Whoever
  fills it honestly — with consent, with sources, without forged authorship —
  sets the standard for the digital era.

## 3. How this differs from "digital avatars"

| Common approach | Our approach |
|---|---|
| Upload chat logs into a cloud model | Everything local: model, archive, voice, biometrics. Cloud only by explicit choice |
| The model "plays the role" of a person | Every statement rests on a recorded event `[event:N]` or a confirmed fact `[fact:N]`; a fabricated reference is rejected by code |
| Personality is the result of a prompt | Personality is the result of consent, source verification, a correction loop and a signed, dated slice |
| One static avatar | Yearly slices, restorable as separate copies, with no leakage of the "future" into the past |
| Anyone can talk "as" the person | The copy does not activate on a stranger's voice and does not answer outsiders on the owner's behalf |
| Text and only text | A body: camera on a pan/tilt turret, microphone, owner recognition by face and voice, timestamped vision events |

## 4. What already works (verifiable, October 2026)

These are not slides. The public part (concept, Mind ABI, Fly Lab, roadmap) is
on GitHub: https://github.com/MyFirstProd/PAIStation. Working code, tests and
reports live in the author's private repository, available under NDA; the
numbers below come from the latest run.

**Body**
- Pan/tilt turret: Arduino Uno R3, two MG996R servos, working range 5–175° on
  both axes, smooth acceleration profile (50°/s²) as the primary safety
  parameter; calibration in EEPROM; FHD USB camera; Razer Seiren Mini microphone.
- Software face tracking with jitter suppression (EMA) and protection against
  instant loss of privileges when the head turns.
- Local owner recognition by face and voice; the result is always
  probabilistic and carries `verified: false` — recognition is **never** treated
  as proof of authorship or consent.
- **Hands and keyboard.** The system already has a computer-control loop:
  code, browser, Windows desktop. It works **with a mouse and keyboard the way
  a human does** — sees the screen, moves the cursor, types — not through
  hidden APIs; irreversible actions require confirmation. 13 of 20 acceptance
  criteria for this loop are closed. In the memory profile the hands are
  deliberately disabled so that memory is finished first; later the copy will
  be able to work at a computer the way the owner did — with his sequence of
  actions and his habits.

**Memory**
- Encrypted event archive (SQLite, Windows DPAPI): each event is an envelope
  with sensor time, receipt time, timing quality and provenance. Idempotent,
  protected against duplicates and reordering; source outages are visible.
- Conversational memory with sources: the model receives only trusted excerpts
  with identifiers; references are checked after the answer.
- **Facts with validity periods**: "Remember / Correct / Forget" run without
  the model; a correction never overwrites the old value but builds a version
  chain. Statements that look like passwords or keys are rejected.
- **Speaker attribution** on every heard utterance: typed, voice-matched
  against a reference (with similarity), or "not analyzed".
- Verbatim recall without the model ("Recall: …" → quotes with time and source).
- Time windows in questions ("yesterday", "last week", "3 days ago").

**Slices (tree rings)**
- A full encrypted archive slice with manifest, integrity verification and
  restore **into a separate copy** with all sensors disabled. A real archive of
  3,902 records has been saved and restored; the live archive stayed untouched.
- A "future leak" check: a fact added after the slice must not appear in the
  restored copy — this is an automated test.

**Quality and engineering gates**
- 2,211 automated tests; static analysis and type checks clean.
- Gate M0 closed (replay with duplicates, reordering, clock jumps, audio outage,
  restart — 23 named checks).
- A **one-hour memory trial** harness is built: 6 stages, 20 facts,
  4 corrections, guest utterances, slice and restart, 30 deferred questions
  (facts / corrections / time / "no evidence" / authorship / vision), threshold
  27/30 plus hard requirements: zero fabricated references, no guest words
  attributed to the owner. Synthetic run: 30/30; the live hour on a real
  microphone is the immediate next step.
- Mind ABI v0 — a protocol separating six concepts: identity, memory,
  cognitive state, model/connectome, embodiment, continuity log. It lets a
  language model and a neural brain model attach to the same body and the same
  memory as interchangeable "thinking" modules.

What is **not** done, stated plainly: the live one-hour and 24-hour runs,
portable restore onto another machine, episodic consolidation, training of a
personal adapter, and any fly neural backend.

## 5. How the system learns and listens

1. **The body observes.** The turret camera keeps the owner in frame; the
   microphone records with timestamps. Every frame and phrase becomes an event
   with source, time and uncertainty. No model rewrites observations with its
   own descriptions.
2. **Consent comes before recording.** Sources are enabled explicitly by the
   owner; revoking consent cancels operations already in flight. On first
   launch every source is off.
3. **Attribution.** Each utterance carries who probably said it. A guest's
   voice never becomes "the owner's opinion".
4. **The companion-interviewer.** A second entity on the same corpus — not a
   copy but a conversational partner that asks at the right moment: "you
   mentioned your grandfather — tell me about him." The answer enters the corpus
   whole, with audio. Declining is a normal answer; the topic is not pressed.
5. **Facts and corrections.** The person says "Remember", "Correct", "Forget" —
   the system keeps a version chain without erasing history. This is how a fact
   acquires a validity period: "in 2026 I loved X; from 2029 I did not."
6. **Episodes and consolidation (next stage).** Episodes with participants and
   source references are built from the event stream; overnight consolidation
   produces derived records, never replacements for evidence.
7. **The slice.** Periodically a signed, dated slice is created: memory, style,
   voice corpus, the chosen model and its parameters, limits of representation.
   The slice is restorable as a separate copy.
8. **Training comes last.** A personal adapter (speech manner, reactions) is
   trained only once the data has passed quality checks and independent tests
   exist. Weights never replace sources inside a slice.

## 6. Research program: from fly to human

This is the main reason the project deserves funding as research rather than as
an app.

**Hypothesis.** If the *mechanism of thinking* (language model, neural
simulation, anything) is separated from the *personality* (memory, identity,
embodiment, continuity in time), the same memory and the same body can be
attached to different mechanisms — and we can measure what is preserved.

**Stage A. A full fly connectome drives the body.** Open data (FlyWire,
MaleCNS) and open executable kernels (DOOMFLY, Shiu et al., Flyvis) make an
honest experiment possible: camera input changes the neural network's state,
neuron output turns the turret, the turn changes the next frame. No language
model steers in secret. Criteria are declared in advance: ≥ 90% direction
accuracy on held-out scenes, p95 ≤ 100 ms, mandatory controls — blinded input,
silenced readout, shuffled connectivity. If the neural path contributes nothing
measurable under ablation, that is an honest negative result.

**Stage B. Hybrid.** Through Mind ABI the neural controller proposes where to
look, the language model proposes what to say, and a host arbiter decides. Four
conditions are compared: baseline controller only, neurons only, model only,
hybrid.

**Stage C. Transferring decisions.** Part of emotional-behavioral reactions
(orienting, alarm, interest, avoidance) are sensorimotor circuits that a fly
has too. We test whether such reactions can be moved out of the language model
into a scanned brain while memory and speech stay outside. This is the first
rehearsal of the central question: **what remains "me" if the mechanism is
replaced**.

**Stage D. Readiness for a human scan.** A complete human connectome does not
exist today, and we do not promise one. But all of the engineering — the
protocol, the continuity log, slices, the separation of memory from mechanism,
consent ethics — is built so that when such data appears we already have
proven infrastructure, a corpus spanning years and an evaluation method. Then
an "emotionally remembering copy" becomes an engineering task, not a fantasy.

Every claim in this section is a working hypothesis with pre-declared failure
criteria. We do not use the phrase "mind upload" as a fact.

## 7. Intermediate stage: digital immortality for a pet

A dog or a cat is the ideal first "personality" for the method:

- the same sensors (turret camera, microphone), the same events, the same slices;
- an animal's behavior is easier to measure than a human's, and the ethical
  stakes are lower;
- the emotional value for a family is enormous while expectations are more
  honest: nobody expects philosophy from a copy of a dog — they expect
  recognition, reaction, voice;
- the pet is exactly where transferring sensorimotor reactions onto neural
  models (Stage C) is convenient to test.

It is at once a scientific test bed and the first understandable product that
can be shown to families before the human copy matures.

## 8. Ethics built into code, not into a policy page

- Recording only with explicit consent; revocation takes effect immediately.
- Recognition ≠ authorship ≠ permission. Three different things, three fields.
- The copy does not answer outsiders on the person's behalf and does not
  activate on a stranger's voice.
- Every answer of the copy rests on a source; "I don't know" is a mandatory
  honest answer.
- A slice is signed and dated; a "new version" is never silently presented as
  "the same person" — the transition is recorded in the continuity log.
- How the copy passes to descendants (medium, conditions, who may activate it)
  is a separate charter the family approves during the person's lifetime.

## 9. Roadmap (24 months)

| Period | Milestone | What it proves |
|---|---|---|
| 0–3 mo | Live one-hour and 24-hour memory runs; portable slice restore | Memory survives restarts, a full day and a change of machine |
| 3–6 mo | Episodes and consolidation; the founder's first yearly slice ("ring 2026") | "What happened and what changed" can be asked with sources |
| 3–9 mo | Fly Lab Stage A: full fly connectome drives a virtual, then a real turret | The neural loop contributes measurably (or honestly does not) |
| 6–12 mo | Personal adapter for speech manner and voice with independent tests | The copy "sounds like" without losing source honesty |
| 9–15 mo | "Digital pet" pilot in 3–5 families | The method transfers to another personality; first product |
| 12–18 mo | Mind ABI hybrid (Stage B) and reaction transfer (Stage C) | What is preserved when the mechanism is replaced |
| 18–24 mo | Copy pilot with 5–10 volunteers, legacy transfer charter, publications | Reproducibility, an ethical standard, a scientific result |
| 18–24 mo | Return of the "hands": the copy works at a computer with mouse and keyboard under supervision, reproducing the owner's habits | Personality shows not only in words but in actions |

## 10. What it takes to do this well and production-grade

Today the project is built by one person, in the evenings, on a consumer PC
with an RTX 3060 (12 GB) and a hard drive. That is enough for a prototype and
not enough for research: the fly neural model, adapter training, 24-hour
video/audio recording and several personalities at once all hit GPU memory and
disk limits.

### 10.1 People

The founder moves to the project **full time**. That requires closing current
personal obligations that now force his time to be split, and securing income
for the research period.

Amounts below are in US dollars; ruble amounts are converted at the Bank of
Russia rate for October 6, 2026 (84.93 ₽/$, rounded to 85).

| Item | 12-month estimate | Note |
|---|---|---|
| Closing the founder's existing debts | **≈ $8.2k** (700,000 ₽) | One-time fixed amount; documented |
| Founder salary (full-time) | **≈ $70.6k** (500,000 ₽/month × 12 = 6,000,000 ₽) | The founder currently earns 250,000 ₽/month on outside work; the 2× rate compensates giving up all other contracts for full-time work on the project |
| Ethics / legal advisor (hourly) | ≈ $5–10k | Legacy charter, consents, data licenses |

No second engineer is budgeted for the first year: working full time, the
founder delivers the roadmap through month 12 alone; growing the team is a
decision made on the results of the first milestones.

### 10.2 Server and equipment (estimates; October 2026 prices are indicative)

| Component | Purpose | Estimate |
|---|---|---|
| Workstation: 1× RTX PRO 6000 (96 GB) **or** 2× RTX 5090 (2×32 GB), 128–256 GB RAM | Fly neural model + language model + adapter training simultaneously | $15–25k |
| Storage 40–60 TB (RAID, encrypted) + offline backup | Raw audio/video over several years (≈ 2.8 GB/day for 16 kHz audio alone; video an order of magnitude more) | $4–8k |
| 2–3 "body" kits (turret on industrial servos, camera, microphone, microcontroller) | Pet pilot and volunteers | $1.5–3k per kit |
| UPS, network, rack | Continuous 24/7 recording | $2–3k |
| Cloud GPUs for short experiments (no personal data) | Neural simulation runs on different hardware | $5–10k / year |

### 10.3 Premises

Work can be done from home, but continuous recording and a 24/7 server need a
dedicated space: sound insulation, stable power, room for a rack and 2–3
turret stands, and a place to receive volunteers and families with pets.

| Item | 12-month estimate |
|---|---|
| Rent for a small lab/office (30–50 m²), Moscow | **≈ $28–42k** (200,000–300,000 ₽/month × 12) |
| Fit-out, sound insulation, electrical | $5–10k |

### 10.4 Summary

| Scenario | 12-month total | What we get |
|---|---|---|
| **Minimal** (founder full-time, one workstation, working from home) | debts $8.2k + salary $70.6k + equipment $21–36k + ethics/legal $5–10k = **≈ $105–125k** (≈ 9–10.5 M ₽) | Milestones 0–9 mo: live memory, yearly slice, Fly Lab Stage A |
| **Research** (+ Moscow lab, + body kits, + cloud, + pet pilot) | Minimal + rent $28–42k + fit-out $5–10k + 2–3 body kits $3–9k + cloud GPUs $5–10k = **≈ $145–195k** (≈ 12.5–16.5 M ₽) | Full roadmap to month 15, first families, publications |

Ruble items (debts, salary, rent) are actual obligations and Moscow market
rates; dollar items are engineering estimates with headroom, not vendor quotes.

## 11. Why this deserves funding

Not for quick profit. For what grows out of the ideas:

- **A standard for honest digital memory.** Consent, source, time, attribution,
  slices — this is a protocol every "digital twin" product will need. We are
  building it first and in the open.
- **Mind ABI as a bridge between AI and neuroscience.** One contract to which a
  language model, a fly connectome and, one day, a human scan can attach.
- **The Theseus experimental method.** A measurable answer to "what remains a
  personality when the mechanism is replaced" is a scientific result that does
  not exist yet.
- **Legacy as a new category.** Families gain something history never had: the
  ability to ask a great-grandfather for advice in his own voice, with his own
  memory, and with an honest marker where the evidence ends.

Commercial paths exist and are obvious — a legacy service for families, a
"digital pet" product, licensing the memory core to companion developers — but
they follow the research rather than lead it.

## 12. What we ask

1. Funding for one of the two scenarios in Section 10 for 12 months against the
   verifiable milestones in Section 9.
2. Access to the developer and neuroscience community for review of the
   Fly Lab methodology.
3. The right to keep the memory core and the Mind ABI protocol open; the
   participants' personal data remains their property, always.
4. A direct conversation with the founder before a decision: a live demo of
   the working system (camera, memory, slice) takes 30 minutes and says more
   than any document.

The project is run transparently: every milestone is a test, a report and a
dated slice that can be verified — not a presentation.

---

**Author and contacts**

Alexander D. Budanov — founder and sole developer of the project.

Email: it@alexbudanov.ru · Telegram @alexandr_budanov_it · phone on request

GitHub: https://github.com/MyFirstProd/PAIStation · Author's channel (projects, hardware, work in progress): https://t.me/error404_engineer_not_found

*The working repository and technical reports are provided by the author on request under
NDA; the owner's personal data, biometrics and voice are never shared under any
circumstances.*

*© 2026 A. D. Budanov. Document v1.0, October 6, 2026. The current version is
published in the author's GitHub repository linked above.*

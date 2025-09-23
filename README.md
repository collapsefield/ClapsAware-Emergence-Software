# Collapse Aware AI — Emergence & Bias (CAAI-Emergence-Software)

**Public repository (redacted materials) — Collapse Aware AI / Verrell’s Law**

**Short tagline:** Emergent, history-biased NPC middleware & research resources (public, redacted).

---

## Overview

**Collapse Aware AI (CAAI)** is a middleware + research program that applies *history-dependent* biasing (Verrell’s Law) to produce persistent, emergent behaviour in simulated agents and environments.

In plain terms: CAAI gives game worlds a short, tunable memory. Rather than every NPC, object, or location behaving only from the current tick, local histories and recorded events subtly bias future choices and states — producing *lived-in* persistence across scenes. The middleware is designer-facing (high-level knobs only in the public materials); implementation and integration details are available under controlled collaboration.

**Examples**
- **NPC routine formation** — a guard who repeatedly finds loot at a location revisits and shifts patrols, not because it was scripted but because the world “remembers” the event.  
- **Place memory** — paths wear where players walk; campfires attract repeat visitors; abandoned camps accumulate traces that influence future AI choices and spawning.  
- **Social drift** — emergent groups form/dissolve as interactions reinforce or erode affiliations; reputations and local histories shape willingness to trade or assist.  
- **Economies & persistence** — marketplaces and resource nodes shift value as repeated behaviours bias supply/demand and NPC priorities.  
- **Designer control** — persistence strength, decay, and contextual filters exposed as safe, high-level knobs (no core math needed).

This repository is the public, redacted hub for research notes, release materials, and outreach assets. Implementation-level code, runnable toy models, parameter sweeps, and integration recipes are intentionally withheld and are ava

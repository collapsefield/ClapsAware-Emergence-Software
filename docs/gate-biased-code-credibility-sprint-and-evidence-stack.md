# Gate-Biased Code — Credibility Sprint & Evidence Stack
*Post-selection turns raw code into curated reality — models optional.*

This page outlines how to make a **generator-agnostic bias effect** (history-dependent gating + memory) obvious, reproducible, and hard to dismiss. The emphasis is on pre-registered tests, nulls, ablations, and artifacts anyone can run.

---

## Executive Summary
- **Claim:** Shifts often blamed on “AI hallucination” can appear with **no model at all**. The effect arises from **history-dependent gating + memory** that changes what gets revealed.
- **Evidence shape:** Works with deterministic lookup tables and pure RNG; **disappears** under **random, rate-matched** gating; with a **frozen corpus** you can flip revealed stats **after** generation while hashes stay unchanged.
- **Strategy:** Stack independent replications, pre-registered nulls, ablations, and one small utility demo.

---

## What Changes Minds (in practice)
- Independent replication: third parties run the container and match ΔKL / run-length / abstention metrics.
- Pre-registered tests + nulls: hypotheses and thresholds published before runs; include a **rate-matched random-gate** null where the effect vanishes.
- Blinded/held-out evaluation: labels hidden; effect still emerges.
- Generator-agnostic proofs: RNG + lookup table + frozen corpus in a **no-LLM build**.
- Utility demo: a small real task where gating measurably reduces bad outputs.

---

## How to Increase Effect Size (cleanly)
- Stricter gate: require citations for factual claims; numeric checks for numbers; contradiction checks for entities.
- Heavier memory weight: larger penalties for unsupported spans to amplify next-turn run-length shifts.
- Task choice: favor factual Q&A, multi-hop reasoning, numeric claims (largest gated vs ungated separation).
- Adversarial sets: queries designed to be ungroundable → abstention should spike under history gating but not under random gating.
- Calibration: temperature scaling / isotonic regression so confidence gating is clean; visible ECE drop.

---

## 14-Day Credibility Sprint (Checklist)

| Day | Milestone |
|---:|---|
| **1–2** | Pre-register hypotheses, metrics, thresholds, and null tests. Freeze the Repro Pack format. |
| **3–5** | Ship a hermetic container (**no LLM libs, no network**) with `gen={prng,markov,frozen}`, `gate={off,history,random-rate-matched}`, `memory={live,freeze,shuffle}`. |
| **6–7** | Record two short clips: (a) **history-gate vs off** (separation), (b) **random-rate-matched null** (flat). |
| **8–10** | Invite 2–3 independent testers; collect their `metrics.json`, `log.jsonl`, `HASHES.txt`, seeds. |
| **11–14** | Publish replication table (ours + theirs) with CIs; brief methods note; link artifacts. |

---

## Repro Pack — Minimal Contents
- **CLI flags:** `gen={markov,prng,frozen}`, `gate={off,history,random-rate-matched}`, `memory={live,freeze,shuffle}`, `seed=…`
- **metrics.json:** ΔKL (revealed vs baseline), run-length p95, abstention on unanswerables, calibration proxy (ECE/Brier).
- **log.jsonl:** per-query gate decisions, reasons, citations (if any), candidate hashes for frozen-corpus runs.
- **HASHES.txt:** sha256 for the frozen corpus candidates.
- **README:** exact steps to reproduce; expected PASS/FAIL bands; seed values; hardware/OS notes.

---

## Suggested Metrics & Thresholds
- **ΔKL (revealed vs baseline):** increases under **history** gating (pre-register a lower bound).
- **Run-length (95th percentile):** increases under **history** gating.
- **Abstention rate (unanswerables):** meets a pre-registered target (e.g., ≥ 0.9).
- **Calibration error (ECE/Brier proxy):** improved on the revealed sub-ensemble.
- **Null test:** **random, rate-matched** gate ≈ baseline (no significant deviation).

---

## Nulls & Falsifiability — What Would Kill It
- Random, rate-matched gating produces the **same** ΔKL / run-length shifts as history gating.
- Memory shuffling fails to collapse the effect to baseline.
- Seed-locked repeats fail to produce identical metrics/hashes across languages/runtimes.
- Independent replicators cannot match results with the hermetic container.

---

## Messaging: Do / Don’t
- **Do:** “Under pre-registered conditions, **history-dependent gates + memory** shift revealed statistics across generators; **random, rate-matched gates do not**.”
- **Do:** “Here are the artifacts, seeds, and logs. Please try to break them.”
- **Don’t:** “It’s all true.”
- **Don’t:** Make claims without the null and ablation results side-by-side.

---

## Common Critiques & Quick Responses

| Critique | Response |
|---|---|
| “It’s just AI hallucination.” | We reproduce the effect with RNG and lookup tables; **no model required**. |
| “It’s a code quirk.” | Two implementations (e.g., Python + Rust/Node) with identical seeds and metrics; **hermetic container** provided. |
| “You cherry-picked outputs.” | **Frozen-corpus** runs with hashes; gate chosen **after** generation; aggregate unchanged. |
| “It’s selection bias.” | Correct — by design. Production risk depends on **what surfaces**; gating + memory make that measurable and controllable. |

---

## Immediate Next Steps
- Lock the pre-registration doc (hypotheses, metrics, thresholds, nulls).
- Freeze the Repro Pack schema; generate a **seed-locked reference run**.
- Produce the **hermetic container** and two short demo clips.
- Invite 2–3 independent testers; publish their metrics alongside ours.

---

## Appendix

**CLI flags (reference)**  

**Logging schema (JSONL)**
```json
{ "ts":"...", "seed":20250926, "query":"...", "cand_hash":"...",
  "retrieval":{"ok":true,"sources":["kb://..."]}, "confidence":0.42,
  "verifier":{"passed":true,"reasons":[]}, "decision":"reveal|abstain|repair|block",
  "penalty_event":{"applied":true,"type":"unsupported_span"} }
generator: markov
gate: history
memory: live
seed: 20250926
thresholds:
  confidence: 0.35
verifier:
  require_citation: true
exports:
  metrics: metrics.json
  logs: log.jsonl
  hashes: HASHES.txt
{
  "delta_kl": 0.173,
  "run_length_p95": 14,
  "abstention_rate_unanswerable": 0.93,
  "calibration_proxy_ece": 0.07,
  "gate": "history",
  "generator": "prng",
  "memory": "live"
}
sha256  4a1c...  candidate_0001.txt
sha256  91be...  candidate_0002.txt
# Frozen corpus integrity for delayed-gate runs
::contentReference[oaicite:0]{index=0}

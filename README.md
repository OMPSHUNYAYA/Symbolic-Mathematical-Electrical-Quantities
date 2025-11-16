# SSMEQ — Shunyaya Symbolic Mathematical Electrical Quantities
*Turn ordinary electrical data into a zero-centric, replayable health signal.*

![GitHub Stars](https://img.shields.io/github/stars/OMPSHUNYAYA/Symbolic-Mathematical-Electrical-Quantities?style=flat&logo=github) ![License](https://img.shields.io/badge/license-Open%20Standard%20%2F%20Open%20Source-brightgreen?style=flat&logo=open-source-initiative)

**Executive overview**  
SSMEQ is a small, manifest-first layer that rides beside existing electrical telemetry and turns ordinary volts, amps, and watts into zero-centric, self-checking records — without touching your meters or SCADA. It adds symbolic contrasts `e_V`, `e_I`, `e_f`, `e_P`, a power residual `r_P`, and health bands `band_P`, then optionally commits to a canonical subset (`sha256=<hex>`) and links records with a portable continuity stamp (`SSMCLOCK1|<iso_utc>|nonce=<...>|sha256=<...>|prev=<...>`). *Observation-only.*

---

**Key benefits**
- **Immediate clarity from ordinary signals** — SSMEQ turns everyday volts, amps, watts, and power-factor into expressive zero-centric contrasts (`e_*`) that highlight stability, drift, and imbalance with far more sensitivity than raw values alone.
- **Self-consistency check for electrical health** — The power residual `r_P` exposes mismatches between V, I, pf, and P that often hide wiring issues, sensor degradation, or load anomalies.
- **No meter changes or SCADA rewrites** — SSMEQ is fully additive. Your original electrical measurements remain untouched (`phi((m,a)) = m`). SSMEQ rides beside them as extra columns, tags, or JSON keys.
- **Operational insight without installing new hardware** — Detect soft faults, phase shifts, mis-calibration, and load instability using data you already collect.
- **Replayable truth via evidence chains** — Optional canonical subset commitments (`sha256`) and continuity stamps create tamper-evident electrical histories, useful for audits, compliance, digital twins, and root-cause investigations.
- **One symbolic standard across domains** — SSMEQ fits into the broader Shunyaya symbolic ecosystem, so the same evidence scripts and manifest discipline work for networks, temperature, chemistry, and more.

---

## Quick Links
- **Docs:** [`docs/SSMEQ_ver2.0.pdf`](docs/SSMEQ_ver2.0.pdf) • [`docs/Brief-SSMEQ_ver2.0.pdf`](docs/Brief-SSMEQ_ver2.0.pdf)
- **Specs:** [`specs/README_PUBLIC_SSMEQ.txt`](specs/README_PUBLIC_SSMEQ.txt) • [`specs/CANONICAL_SUBSET_RECIPE_SSMEQ.txt`](specs/CANONICAL_SUBSET_RECIPE_SSMEQ.txt) • [`specs/GETTING_STARTED_SSMEQ.txt`](specs/GETTING_STARTED_SSMEQ.txt) • [`specs/OUTPUT_EXAMPLES_SSMEQ.txt`](specs/OUTPUT_EXAMPLES_SSMEQ.txt) • [`specs/STAMP_SPEC_SSMEQ.txt`](specs/STAMP_SPEC_SSMEQ.txt)
- **Manifests:** [`manifests/EFFECTIVE_MANIFEST_SSMEQ_v2_0.json`](manifests/EFFECTIVE_MANIFEST_SSMEQ_v2_0.json) • [`manifests/MANIFEST_AND_BANDS_SSMEQ.txt`](manifests/MANIFEST_AND_BANDS_SSMEQ.txt)
- **Evidence:** [`evidence/SSMEQ_evidence_v2_0.zip`](evidence/SSMEQ_evidence_v2_0.zip)

---

## Core Definitions (ASCII)

**Payload invariance**  
`phi((m,a)) = m`  
Raw electrical measurements stay exactly as they are; SSMEQ only adds a symbolic lane.

**Zero-centric lenses (contrasts)**  
`e_V := ln( V_rms / V_ref )`  
`e_I := ln( I_rms / I_ref )`  
`e_f := ln( f / f_ref )`  
`e_P := asinh( P / P_scale )`  

At anchors: `e_Q = 0`. Above anchors: `e_Q > 0`. Below anchors: `e_Q < 0`.

**Power residual (self-consistency check)**  
`r_P := e_P_meas - ( e_V + e_I + ln( pf / pf_ref ) )`  

Small, stable `|r_P|` suggests a self-consistent voltage–current–power–pf quartet; drift or jumps can reveal wiring, metering, or modeling issues.

**Bands as duty (policy)**  
`band_P` is a label such as `"GREEN"`, `"AMBER"`, `"RED"` derived from conditions over `e_*`, optional dials (for example `a_pf`, `a_THD`), and `r_P` under a declared `manifest_id`.

**Canonical subset commitment (optional but recommended)**  
A compact, stable byte recipe over key SSMEQ fields is committed as:  
`sha256 := SHA256(bytes)`  
where `bytes` is a newline-joined, UTF-8 encoded canonical subset (for example `timestamp`, `channel_id`, `e_V`, `e_I`, `e_P`, `r_P`, `band_P`, `manifest_id`).

**Continuity stamp (portable timeline link, optional)**  
`SSMCLOCK1|<iso_utc>|nonce=<...>|sha256=<hex>|prev=<hex|NONE>`  
Each record links to the previous one by setting `prev = sha256(previous_canonical_subset)`, making reordering and deletion visible in evidence chains.

---

## License / Usage

**Open standard • Open source** — Free to implement with no fees or registry, provided strictly *as-is* with no warranty or guarantee of fitness for any purpose.  
**Minimum citation** — When adapting or deploying, cite **"Shunyaya Symbolic Mathematical Electrical Quantities (SSMEQ)"** as the concept origin.  
**Non-exclusivity** — Implementations are independent; no party may claim exclusive ownership, endorsement, or stewardship of SSMEQ or its symbolic patterns.  
**Integrity requirement** — Preserve the defined meanings of `phi((m,a)) = m`, the zero-centric lenses `e_V`, `e_I`, `e_f`, `e_P`, the residual `r_P`, manifest-governed band logic `band_P`, and the continuity stamp shape `SSMCLOCK1|<iso_utc>|nonce=<...>|sha256=<hex>|prev=<...>`. If you change any of these, declare the changes clearly in your manifest and documentation.  
**Safety** — SSMEQ is an *observation-only* overlay. Do not use SSMEQ as the sole gating mechanism in life-critical or safety-critical systems without independent verification, redundancy, and domain-appropriate engineering controls.

---

## Semantic Category / Practical Scope

- **Domains:** Power plants, industrial loads, data centers, renewable integration, microgrids, utilities, protection and relay testing, SCADA and historian analytics, grid-edge devices.  
- **What SSMEQ adds:** A zero-centric symbolic lane (`e_*`, `r_P`, `band_P`, `manifest_id`, optional `sha256`, optional `stamp`) that turns ordinary volts/amps/watts into a replayable health signal without changing existing measurements.  
- **Integration surfaces:** CSV exports, historians, SCADA streams, message buses, IoT gateways, edge controllers, cloud analytics pipelines. SSMEQ fields ride as extra columns, tags, or JSON keys.  
- **Implementation footprint:** Simple to prototype in Python, Go, Rust, C, or browser/server JavaScript. No protocol rewrites; deployment is additive and can start as a sidecar pipeline.  
- **Relation to the broader Shunyaya stack:** SSMEQ reuses the same zero-centric lens philosophy, manifest discipline, and stamp pattern as other Shunyaya projects, so a single evidence script style can audit multiple domains.

---

## Topics

Shunyaya Symbolic Mathematical Electrical Quantities (SSMEQ), zero-centric electrical telemetry, volts/amps/watts contrasts, `e_V`, `e_I`, `e_f`, `e_P`, power residual `r_P`, health bands `band_P`, manifest-governed policy, canonical subset commitment `sha256=<hex>`, continuity stamp `SSMCLOCK1|...|sha256=<hex>|prev=<...>`, evidence bundles, grid observability, SCADA enhancement, auditability, observation-only symbolic framework.

---


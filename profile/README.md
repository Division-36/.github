<div align="center">

<img src="https://planck-99.pages.dev/logo.png" width="64" height="64" alt="Division-36" />

# Division-36

**On-device security for embedded Linux.** No cloud. No GPU. No dependencies.

[planck-99.pages.dev](https://planck-99.pages.dev) · [LinkedIn](https://linkedin.com/company/division-36) · [zs.01117875692@gmail.com](mailto:zs.01117875692@gmail.com)

</div>

---

We build security for the layer nobody else covers: firmware and embedded Linux on
IoT and OT devices, where antivirus is too large to run and the cloud is not
reachable. Every result is deterministic and carries its own audit trail — which is
what the EU Cyber Resilience Act asks for from August 2027.

## Products

**Planck-99** — behavioural malware classifier, as a closed-form int8 dot product on device.

`37 KB` binary · `34 ns` median · `29.4 M` inferences/sec on one core · `96.28%` on unseen IoT malware (2016–2026) · zero dependencies

→ [Planck-99_PublicBenchmarks](https://github.com/Division-36/Planck-99_PublicBenchmarks)

**Axiom-WAF** — logic-first web application firewall; learns attack structure instead of accumulating rules.

`99.50%` accuracy on 313K samples · `MCC 0.9712` · 199 KB smallest build, trained on 200 samples

→ [AxiomWAF-Brain_PublicBenchmarks](https://github.com/Division-36/AxiomWAF-Brain_PublicBenchmarks)

**SYRTH** — risk-surface mapper for Python. *Traces, not verdicts.*

SYRTH does not decide whether code is vulnerable. For every dangerous call a source
reaches, it reports the nearest vulnerability class with the ordered evidence chain
and a likelihood — then verifies its own patches by re-analysing them. `--threshold`
marks a finding; it never deletes one.

| Metric | Value |
|---|---|
| Classes · sinks · sanitisers | 11 CWEs · 15 sinks · 66 sanitisers |
| Category coverage | **95.7%** (111/116), 350 records from 24 repositories |
| Rank agreement · rename invariance | **100%** (22/22) · **100%** (350/350) |
| Patch kill rate | **89.0%** (89/100) |
| Precision / recall | none published — there is no negative class |
| Runtime | tree-sitter only · no network · 624 tests |

Ground truth comes from an independent AST scan, never from corpus labels.

→ [Division-36/Syrth](https://github.com/Division-36/Syrth) · MIT · [release v1-prod](https://github.com/Division-36/Syrth/releases/tag/v1-prod)

## Systems tooling

**Z-Jail** — multi-layer Linux sandbox for native code execution. Seven ordered
isolation layers: rlimits, namespaces, `pivot_root`, `NO_NEW_PRIVS`, capability drop,
seccomp-BPF whitelist, BLAKE2b audit trail. ~81 KiB static PIE, C99, MIT.

**Z-Privesc** — Linux privilege-escalation auditor. 17 probes, deterministic verdicts,
2.65 s full scan. Static C binary, zero dependencies, MIT.

**mcOS** — deterministic safety-critical RTOS with a bounded-time AI runtime. Zero-heap
allocation, compositional WCET analysis, fault containment regions, models
hot-swappable under 5 µs. Targets DO-178C, IEC 61508 and ISO 26262 on Cortex-M via the
Zephyr HAL.

## Repository index

| Repository | Contents |
|---|---|
| [Syrth](https://github.com/Division-36/Syrth) | Source, tests, docs — MIT, release `v1-prod` |
| [Z-Jail](https://github.com/Division-36/Z-Jail) | Linux sandbox — MIT, release `v1.1.0` |
| [Z-Privesc](https://github.com/Division-36/Z-Privesc) | Privilege-escalation auditor — MIT, release `v1.0.0` |
| [mcOS](https://github.com/Division-36/mcOS) | Safety-critical RTOS — release `v1.0.0` |
| [Planck-99_PublicBenchmarks](https://github.com/Division-36/Planck-99_PublicBenchmarks) | Datasets, benchmark scripts, technical paper |
| [AxiomWAF-Brain_PublicBenchmarks](https://github.com/Division-36/AxiomWAF-Brain_PublicBenchmarks) | Confusion matrices, per-attack recall, 313K results |
| [AxiomWAF_PublicBenckmark](https://github.com/Division-36/AxiomWAF_PublicBenckmark) | Initial WAF benchmark data |

Syrth, Z-Jail, Z-Privesc and mcOS are public. Planck-99 and Axiom-WAF publish
datasets, results and reproducible scripts only; their source is proprietary.

## Contact

**Ziad Salah** — Founder & Lead Engineer · Giza, Egypt · Pre-seed

Design partners welcome: IoT gateway and OT vendors shipping embedded Linux, ahead of
the CRA deadline.

[zs.01117875692@gmail.com](mailto:zs.01117875692@gmail.com) · [linkedin.com/in/z14d](https://linkedin.com/in/z14d) · [@Zierax_X](https://x.com/Zierax_X)

---

<div align="center"><sub>© 2026 Division-36.</sub></div>

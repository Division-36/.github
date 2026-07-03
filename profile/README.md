<div align="center">

<img src="https://planck-99.pages.dev/logo.png" width="72" height="72" alt="Division-36 logo" />

# Division-36

**On-device security for embedded Linux.**  
No cloud. No GPU. No dependencies. No compromise.

[planck-99.pages.dev](https://planck-99.pages.dev) · [zs.01117875692@gmail.com](mailto:zs.01117875692@gmail.com) · [LinkedIn](https://linkedin.com/company/division-36)

</div>

---

Division-36: Named after 1936 — the foundational year of mathematical computing and Turing's absolute logic.

## What we build

Division-36 builds security software for the layer nobody else covers: the firmware and embedded Linux layer of IoT and OT devices, where traditional antivirus is too large to run and cloud solutions require connectivity that doesn't exist.

Our work is deterministic, verifiable, and designed to satisfy the EU Cyber Resilience Act audit requirements by construction.

---

## Products

### Planck-99 — Behavioral Malware Classifier
> *Syscall-based malware detection for embedded Linux. 37KB. 34 nanoseconds. No dependencies.*

The gap is structural: traditional AV needs 50–500 MB RAM. Embedded Linux devices have 256 KB–4 MB. Planck-99 fills that gap — a closed-form int8-quantized classifier that runs entirely on-device, produces a JSON proof file per classification, and fits in the L1 cache of virtually any embedded Linux SoC.

```
Binary size       37 KB   (27 KB binary + 10 KB ring buffer)
Median latency    34 ns   σ ≤ 14 ns across 9 independent runs
Throughput        29.4 M inferences/sec  ·  single CPU core, no GPU
Accuracy          96.28%  (unseen IoT malware, 2016–2026)
Precision         97.71%
Recall            97.87%
Generalization    51× beyond training trace length — mathematical property, not tuning
Dependencies      None
```

All numbers are reproducible from public datasets and benchmark scripts.  
→ **[Planck-99_PublicBenchmarks](https://github.com/Division-36/Planck-99_PublicBenchmarks)**

---

### Axiom-WAF — Logic-First Web Application Firewall
> *Reasoning-based WAF that learns security through statistical hypothesis testing, not static rules.*

Axiom-WAF reaches the same accuracy (99.5%, MCC 0.9712) whether trained on 200 samples or 60,000. This is what we call Logic Saturation — the model identifies the mathematical laws governing attack patterns, so data volume becomes irrelevant after the hypotheses are discovered.

```
Accuracy (313K test samples)   99.50%
MCC                            0.9712
Axiom-Nano binary              199 KB  (trained on 200 samples)
Axiom-Mini binary              1.01 MB
```

→ **[AxiomWAF-Brain_PublicBenchmarks](https://github.com/Division-36/AxiomWAF-Brain_PublicBenchmarks)**  
→ **[AxiomWAF_PublicBenckmark](https://github.com/Division-36/AxiomWAF_PublicBenckmark)**

---

### SYRTH — AST-Based Vulnerability Detector for Python
> *Parses Python source into security-relevant function traces, classifies them into 8 CWE categories. 99% accuracy. 246 µs per scan. No network.*

Most static analysis tools pattern-match on syntax. SYRTH works differently: it parses Python source files into AST-derived function traces — sequences of security-semantic tokens like `sink:execute`, `arg:user_input`, `@login_required` — then classifies those traces using a mean-pooled embedding model trained on real vulnerability data from the GitHub Advisory Database and OSV. The result is a classifier that understands the *structure* of a vulnerability, not just its surface appearance.

Two inference backends ship together: a Python/PyTorch dev mode and a compiled C engine for production. The C engine compiles to a self-contained shared library with no runtime dependencies beyond libc.

```
Overall accuracy     99.0%   (730 held-out test samples, strict split, no leakage)
C engine latency     246 µs  median  ·  P99 278 µs  ·  σ = 21.8 µs
C engine throughput  4,100 scans/sec
Python latency       802 µs  median
Speedup (C vs Py)    3.3×
Peak RAM (Python)    0.25 MB
Peak RAM (C engine)  5.0 MB
Vulnerability classes  8 CWEs covered
```

**Per-class accuracy on 730 unseen samples:**

| Vulnerability | CWE | Accuracy |
|---|---|:-:|
| SQL Injection | CWE-89 | 96.0% |
| XSS | CWE-79 | 100.0% |
| IDOR | CWE-284 | 98.9% |
| SSRF | CWE-918 | 100.0% |
| Path Traversal | CWE-22 | 98.2% |
| Open Redirect | CWE-601 | 100.0% |
| Broken Auth | CWE-287 | 95.0% |
| Remote Code Execution | CWE-94 | 100.0% |

Benchmarks run across three independent test sets (2K, 7K, 20K samples). Accuracy is consistent at 99% across all three. Latency varies 192–275 µs on the C engine depending on dataset size. All results are reproducible from the benchmark scripts in the public repository.

The model uses a Bag-of-Words approach over security-semantic tokens rather than a Transformer. This is a deliberate design choice: on short, structurally regular sequences of 5–30 tokens, mean-pooled embeddings generalise better than attention mechanisms and require substantially less data to converge.

→ **[Syrth_PublicBenchmark](https://github.com/Division-36/Syrth_PublicBenchmark)**

---

## Why the timing matters

Three forces are converging simultaneously:

| Signal | Detail |
|--------|--------|
| **EU CRA — August 2027** | Every connected device sold in Europe must demonstrate on-device, traceable security. Perimeter-only defense is no longer legally sufficient. |
| **Nation-state IoT attacks** | Flax Typhoon (PRC) built a 200,000-device botnet from compromised embedded systems. FBI disrupted it September 2024. The firmware layer is the confirmed attack surface. |
| **ICS vulnerability acceleration** | 2,155 CVEs across 508 ICS advisories in 2025 — the first year CISA published 500+ ICS advisories. Average CVSS crossed 8.0. |

Planck-99's JSON proof file addresses EU CRA Article 13 traceability requirements by construction. Each classification produces a reproducible audit trail: input vector, weight hash, computed score.

---

## How Planck-99 works

```
Kernel hook  →  Ring buffer  →  32-dim frequency vector  →  Int8 dot product  →  JSON proof file
```

1. A kernel hook captures the syscall trace into a 10 KB ring buffer
2. 32 normalized frequency ratios are extracted — ratios, not raw counts, so the representation is length-invariant by construction
3. A closed-form int8-quantized dot product classifies the trace in 34 ns
4. A JSON proof file is generated: input vector, weight hash, score — complete audit trail

The classifier generalizes to 51× its training trace length because normalization makes the representation independent of trace length. This is a mathematical property, not an empirical result.

---

## Competitive position

|  | Traditional AV | Cloud Detection | Signature Scanner | **Planck-99** |
|--|:-:|:-:|:-:|:-:|
| Peak RAM | 50–500 MB | server-side | ~30 MB | **37 KB** |
| Inference | 10–100 ms | 200–2,000 ms | 1–50 ms | **34 ns** |
| Network required | no | **mandatory** | no | **never** |
| Air-gap safe | ✗ | ✗ | ~ | **✓** |
| MCU-class viable | ✗ | ✗ | ✗ | **✓** |
| Deterministic | no | no | ~ | **✓** |
| CRA audit trail | ✗ | ✗ | ✗ | **✓** |

---

## Repository index

| Repository | Contents | Status |
|------------|----------|--------|
| [Planck-99_PublicBenchmarks](https://github.com/Division-36/Planck-99_PublicBenchmarks) | Datasets, benchmark scripts, technical paper, pitch deck | 🟢 Public |
| [AxiomWAF-Brain_PublicBenchmarks](https://github.com/Division-36/AxiomWAF-Brain_PublicBenchmarks) | Confusion matrices, per-attack recall, 313K test results | 🟢 Public |
| [AxiomWAF_PublicBenckmark](https://github.com/Division-36/AxiomWAF_PublicBenckmark) | Initial WAF benchmark data | 🟢 Public |
| [Syrth_PublicBenchmark](https://github.com/Division-36/Syrth_PublicBenchmark) | AST-based Python vulnerability detector — 8 CWEs, 99% accuracy, benchmark datasets | 🟢 Public |

Source code is proprietary. Public repositories contain datasets, benchmark results, methodology, and reproducible scripts only.

---

## Recognition

- **IEEE AIITA 2026** — accepted paper (PTRR Framework)
- **Nature Portfolio / Scientific Reports** — paper under review (DVF Framework)
- **HackerOne VDP** — Top 90 globally, #9 Egypt (Feb–Jun 2026)

---

## Contact

**Ziad Salah** — Founder & Lead Engineer  
Division-36 · Giza, Egypt · Pre-seed

We are seeking design partners: IoT gateway or OT security vendors shipping embedded Linux devices who want to run a live Planck-99 integration pilot ahead of the EU CRA 2027 deadline. Integration takes hours, not months.

[zs.01117875692@gmail.com](mailto:zs.01117875692@gmail.com) · [linkedin.com/in/z14d](https://linkedin.com/in/z14d) · [@Zierax_X](https://x.com/Zierax_X)

---

<div align="center">
<sub>All benchmark results are public and reproducible. · © 2026 Division-36. All rights reserved.</sub>
</div>

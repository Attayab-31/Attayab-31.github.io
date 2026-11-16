---
title: "Muhammad Attayab Ashraf | AI Systems Research"
description: "Research portfolio of Muhammad Attayab Ashraf, focusing on multi-agent LLM systems, inference serving, capacity-aware scheduling, and reproducible AI research."
lang: en
---

<div align="center">

# Muhammad Attayab Ashraf

**Independent Researcher | AI Systems | Multi-Agent LLMs | Agentic AI**

Research on reliable, capacity-aware, and reproducible AI systems, with a focus on how multi-agent workflows behave under shared inference constraints.

[![Research](https://img.shields.io/badge/Focus-AI%20Systems-6C5CE7?style=flat-square)](#research-interests)
[![Multi-Agent Systems](https://img.shields.io/badge/Research-Multi--Agent%20LLMs-3776AB?style=flat-square)](#featured-research)
[![Reproducibility](https://img.shields.io/badge/Approach-Reproducible%20Research-1682C4?style=flat-square)](#research-principles)

[GitHub](https://github.com/Attayab-31) · [LinkedIn](https://www.linkedin.com/in/muhammad-attayab-ashraf-519562262/) · [Professional Website](https://attayab.automivex.com) · [Email](mailto:attayabpc2@gmail.com)

</div>

---

## About

I am an independent researcher and the Founder & CEO of **Automivex**, based in Lahore, Pakistan. My research interests center on AI systems engineering, multi-agent LLM workflows, inference-serving infrastructures, resource allocation, and reproducible benchmark design.

My work explores the systems questions that emerge when capable AI agents move from isolated experiments into concurrent, resource-constrained applications. I am particularly interested in evaluating how multi-agent workflows perform under shared inference quotas, admission control, queueing pressure, and fairness constraints.

## Research Interests

- Multi-Agent Systems and Agentic AI
- Large Language Model (LLM) Systems and Workflow Orchestration
- Inference Serving, Quota Management, and Resource Allocation
- Capacity-Aware Scheduling and Admission Control
- Queueing, Tail Latency, and Service-Level Goodput
- Multi-Tenant Fairness in Shared AI Infrastructure
- Benchmark Design, Statistical Evaluation, and Reproducible Research

## Featured Research

<div align="center">

### AgentQuotaBench

**Evaluating Multi-Agent LLM Workflows Under Shared Inference Quotas**

*A reproducible benchmark protocol for capacity-aware admission control, deadline-adjusted goodput, and tenant fairness.*

[![Protocol](https://img.shields.io/badge/Category-Research%20Protocol-3776AB?style=flat-square)](#protocol-status)
[![Version](https://img.shields.io/badge/Version-1.1-0A66C2?style=flat-square)](#publication-details)
[![Evidence status](https://img.shields.io/badge/Confirmatory%20experiments-Pending-orange?style=flat-square)](#protocol-status)

</div>

### Research overview

AgentQuotaBench investigates how multi-agent large language model (LLM) workflows behave when concurrent user workflows compete for a shared inference-capacity pool. Multi-agent architectures can improve task completion by parallelizing research, planning, execution, and verification, but the same fan-out concentrates demand on shared inference resources.

The protocol proposes a controlled comparison of four workflow architectures under matched task and resource-accounting rules. It evaluates whether additional agent parallelism can improve isolated-task outcomes without reducing service-level goodput, deadline attainment, or tenant fairness under a fixed quota model.

### Abstract

Multi-agent large language model (LLM) workflows can improve task completion by parallelizing research, planning, execution, and verification. The same fan-out increases demand on shared inference capacity, creating bottlenecks in quota-limited settings. AgentQuotaBench studies this trade-off through a benchmark protocol designed to compare workflow architectures under a common, shared inference quota and to evaluate how quota-aware control changes performance, latency, and fairness.

### Research question

> When many user workflows share a finite inference quota, which workflow architecture delivers the most correct, on-time completions per unit of shared capacity, and can quota-aware admission improve service-level goodput without degrading task quality?

### Experimental conditions

| ID | Workflow | Experimental role |
| --- | --- | --- |
| **S1** | Single-agent | Baseline: one agent completes the task. |
| **S2** | Independent best-of-three | Three independent attempts run on isolated task states; all attempts and aggregation are charged to the same total per-workflow ceiling. |
| **M2** | Planner-executor-verifier | Unconstrained multi-agent baseline: a planner creates a bounded task graph, up to two workers execute independent leaves, and a verifier checks the final state. |
| **M3** | Quota-aware M2 | Same topology as M2, with atomic reservation of request, token, and in-flight capacity before worker dispatch; may wait, reduce worker count, or fall back to sequential execution when the shared quota is insufficient. |

### Workloads and load conditions

| Dimension | Prespecified design |
| --- | --- |
| Workload families | AppWorld API/tool-use tasks and a frozen Terminal-Bench release |
| Task selection | At least 60 unique tasks per workload family, subject to task-level licensing and reproducibility checks |
| Concurrency | 1, 4, 8, and 16 logical workflows |
| Arrival patterns | Steady open-loop and burst |
| Quota profiles | Low (0.5×), medium (1.0×), and high (2.0×) normalized reference capacity |
| Replication | Three independent execution seeds |

The quota multipliers are emulator settings normalized to measured single-agent demand. They are not claims about any provider's commercial plan or published quota tier.

### Primary endpoint and evaluation metrics

The single primary endpoint is **deadline-adjusted goodput**: grader-passing workflows completed before their frozen deadlines per minute of a fixed open-loop measurement window. Workflows submitted after the deadline or failing validation do not count as successful goodput.

Prespecified guardrails are **task success** and **p95 end-to-end latency**. Secondary measures include queue delay, rate-limit responses, retry amplification, token use, cost per successful task, coordination overhead, and tenant fairness outcomes.

For the separate mixed-tenant fairness track, the protocol specifies Jain's fairness index alongside absolute goodput, per-tenant service rates, worst-tenant outcomes, and starvation events.

### Proposed experimental scale

<div align="center">

# 34,560

**Core confirmatory workflow episodes**

60 tasks × 2 workload families × 4 architectures × 4 concurrency levels × 2 arrival profiles × 3 quota profiles × 3 independent seeds

</div>

The mixed-tenant fairness track is additional and is counted separately from the core factorial design.

## Research Principles

**Controlled comparison.** The four architectures are evaluated with matched task instructions, a pinned model/provider configuration, equivalent tools and state snapshots, explicit workflow budgets, and a shared quota ledger.

**Service-level evaluation.** The protocol evaluates complete logical workflows rather than relying only on isolated request counts, token totals, or average latency. Goodput, deadline attainment, and fairness are treated as first-class outcome measures.

**Transparent intervention.** M3 changes worker admission rather than the underlying model or communication representation. It reserves estimated shared capacity before dispatch and records reservation, fallback, and overrun decisions.

**Reproducibility.** The proposed release package includes versioned task manifests, model and quota configurations, arrival and fault schedules, prompts, event schemas, run seeds, sanitized traces, and analysis scripts.

## Protocol Status

This work is a **research protocol and benchmark design**, not a completed empirical evaluation. The manuscript specifies the research questions, confirmatory conditions, workload plan, quota model, and statistical analysis plan, but the full benchmark harness and data collection remain pending.

| Component | Status in protocol v1.1 |
| --- | --- |
| Protocol narrative and hypotheses | Specified |
| Four confirmatory architectures | Specified |
| AppWorld and Terminal-Bench workload plan | Specified; exact task releases, task IDs, and component licenses remain to be frozen and checked |
| Shared quota model and normalized profiles | Specified; exact emulator settings to be frozen after the pilot |
| Quota-aware M3 intervention | Specified |
| Statistical analysis plan | Specified; pilot-based precision analysis remains required |
| Deterministic quota reference-model tests | Nine software tests reported as passing |
| Public timestamped preregistration | Pending in the manuscript's submission-readiness checklist |
| Full benchmark harness and provider adapter | Pending |
| Pilot execution | Pending |
| Confirmatory benchmark execution | Pending |
| Empirical findings and benchmark result tables | Not available |
| Full public code and run-artifact release | Pending |

The nine reported tests cover selected reference-model mechanics only, including token-bucket refill, atomic reservation, concurrency limits, usage reconciliation, lease expiry, retry admission, and deterministic quota accounting. They do not establish benchmark performance or any final empirical claim.

No task-success rates, latency quantiles, goodput values, provider rate-limit rates, confidence intervals, p-values, or cost-per-success findings are claimed. Any future empirical claims must be supported by separate benchmark execution and publication.

## Publication Details

| Field | Detail |
| --- | --- |
| **Title** | AgentQuotaBench: Evaluating Multi-Agent LLM Workflows Under Shared Inference Quotas |
| **Author** | Muhammad Attayab Ashraf |
| **Affiliation** | Independent Researcher; Founder & CEO, Automivex |
| **Document category** | Research Protocol / Benchmark Design |
| **Protocol version** | 1.1 |
| **Archive publication date** | November 16, 2026 |
| **Manuscript date shown in PDF** | November 15, 2026 |
| **Archive / Publisher** | Zenodo |
| **Canonical DOI link** | https://doi.org |
| **DOI** | 10.5281/zenodo.24279037 |
| **Zenodo record URL** | https://zenodo.org |
| **Asset status** | PDF-only protocol on Zenodo; no duplicate PDF upload to GitHub |

## Citation

Suggested citation:

Ashraf, M. A. (2026). *AgentQuotaBench: Evaluating Multi-Agent LLM Workflows Under Shared Inference Quotas*. Research Protocol, Version 1.1. Zenodo.

### BibTeX

```bibtex
@misc{ashraf2026agentquotabench,
  author       = {Ashraf, Muhammad Attayab},
  title        = {{AgentQuotaBench: Evaluating Multi-Agent LLM Workflows Under Shared Inference Quotas}},
  year         = {2026},
  month        = nov,
  doi          = {10.5281/zenodo.24279037},
  url          = {https://zenodo.org},
  howpublished = {Zenodo},
  note         = {Research Protocol, Version 1.1. Published November 16, 2026. Confirmatory experiments pending.}
}
```

DOI: [10.5281/zenodo.24279037](https://doi.org/10.5281/zenodo.24279037)  
Zenodo record: [https://zenodo.org](https://zenodo.org)

## Author

<div align="center">

**Muhammad Attayab Ashraf**  
Independent Researcher | Founder & CEO, Automivex  
Lahore, Pakistan

[![GitHub](https://img.shields.io/badge/GitHub-Attayab--31-181717?style=flat-square&logo=github)](https://github.com/Attayab-31)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Muhammad%20Attayab-0A66C2?style=flat-square&logo=linkedin)](https://www.linkedin.com/in/muhammad-attayab-ashraf-519562262/)
[![Website](https://img.shields.io/badge/Website-Attayab-4A90E2?style=flat-square&logo=googlechrome&logoColor=white)](https://attayab.automivex.com)
[![Email](https://img.shields.io/badge/Email-Contact-EA4335?style=flat-square&logo=gmail&logoColor=white)](mailto:attayabpc2@gmail.com)

</div>

For academic inquiries, research discussion, or potential collaboration, please contact [attayabpc2@gmail.com](mailto:attayabpc2@gmail.com).

---

<div align="center">

*Building and evaluating reliable AI systems through rigorous methodology, transparent reporting, and reproducible research.*

</div>

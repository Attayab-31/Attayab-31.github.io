<div align="center">

# AgentQuotaBench

<strong>Evaluating Multi-Agent LLM Workflows Under Shared Inference Quotas</strong>

A reproducible benchmark protocol for capacity-aware admission control, deadline-adjusted goodput, and tenant fairness.

<strong>Research Protocol | Version 1.1 | November 2026</strong>

[![Research](https://img.shields.io/badge/Research-AI%20Systems-6C5CE7?style=flat-square)](https://github.com/Attayab-31)
[![Protocol](https://img.shields.io/badge/Category-Research%20Protocol-3776AB?style=flat-square)](https://github.com/Attayab-31)
[![Version](https://img.shields.io/badge/Version-1.1-0A66C2?style=flat-square)](https://github.com/Attayab-31)
[![Status](https://img.shields.io/badge/Status-Experiments%20Pending-orange?style=flat-square)](https://github.com/Attayab-31)

[![Python](https://img.shields.io/badge/Python-Reference%20Model-3776AB?style=flat-square&logo=python&logoColor=white)](https://www.python.org/)
[![Archive](https://img.shields.io/badge/Archive-Zenodo-1682C4?style=flat-square&logo=zenodo&logoColor=white)](https://zenodo.org/)
[![Author](https://img.shields.io/badge/Author-Muhammad%20Attayab%20Ashraf-444444?style=flat-square)](https://github.com/Attayab-31)

[Overview](#overview) · [Research Questions](#research-questions) · [Experimental Design](#experimental-design) · [Architecture](#system-design) · [Metrics](#evaluation-metrics) · [Protocol Status](#protocol-status) · [Citation](#citation)

[![Share on X](https://img.shields.io/badge/Share-X-111111?style=flat-square&logo=x)](https://twitter.com/intent/tweet?text=AgentQuotaBench%3A%20Evaluating%20Multi-Agent%20LLM%20Workflows%20Under%20Shared%20Inference%20Quotas%20%7C%20Research%20Protocol%20%7C%20Zenodo%20https%3A%2F%2Fdoi.org%2F10.5281%2Fzenodo.24279037)
[![Share on LinkedIn](https://img.shields.io/badge/Share-LinkedIn-0A66C2?style=flat-square&logo=linkedin)](https://www.linkedin.com/sharing/share-offsite/?url=https%3A%2F%2Fdoi.org%2F10.5281%2Fzenodo.24279037)
[![Share on Reddit](https://img.shields.io/badge/Share-Reddit-FF4500?style=flat-square&logo=reddit)](https://www.reddit.com/submit?url=https%3A%2F%2Fdoi.org%2F10.5281%2Fzenodo.24279037&title=AgentQuotaBench%3A%20Evaluating%20Multi-Agent%20LLM%20Workflows%20Under%20Shared%20Inference%20Quotas)
[![Share on WhatsApp](https://img.shields.io/badge/Share-WhatsApp-25D366?style=flat-square&logo=whatsapp)](https://api.whatsapp.com/send?text=AgentQuotaBench%3A%20Evaluating%20Multi-Agent%20LLM%20Workflows%20Under%20Shared%20Inference%20Quotas%20%7C%20https%3A%2F%2Fdoi.org%2F10.5281%2Fzenodo.24279037)

</div>

---

## Overview

AgentQuotaBench is a research protocol designed to evaluate multi-agent large language model (LLM) workflows under shared inference quotas.

Multi-agent systems distribute work across planners, researchers, executors, and verifiers. Although this parallelism can improve individual task completion, it also increases demand on shared inference capacity.

When multiple logical workflows execute concurrently, their agents compete for request-per-minute (RPM) limits, token-per-minute (TPM) limits, in-flight request slots, and retry budgets.

AgentQuotaBench investigates whether this increased inference demand can reduce system-level goodput, increase tail latency, and create unfair resource allocation across tenants.

The proposed benchmark compares four workflow architectures under a common account-level capacity pool and evaluates quota-aware admission control as an intervention.

<strong>Repository:</strong> [Attayab-31/Attayab-31.github.io](https://github.com/Attayab-31/Attayab-31.github.io)

<strong>Publication archive:</strong> [Zenodo](https://zenodo.org) • DOI: [10.5281/zenodo.24279037](https://doi.org/10.5281/zenodo.24279037)

<strong>Portfolio:</strong> [attayab-31.github.io](https://attayab-31.github.io/)

## Abstract

Multi-agent large language model (LLM) workflows can improve task completion by parallelizing research, planning, execution, and verification. The same fan-out concentrates demand on shared inference capacity and can create quota bottlenecks under concurrent workloads.

AgentQuotaBench studies this trade-off through a benchmark protocol designed to compare workflow architectures under a common shared inference quota and to evaluate how quota-aware control changes performance, latency, and fairness.

## Research Questions

The benchmark investigates five research questions.

| ID | Research question |
|---|---|
| RQ1 | How does agent fan-out affect successful throughput and p95 latency as concurrent demand approaches shared inference quotas? |
| RQ2 | How do single-agent, best-of-three, and collaborative architectures behave under matched task and system budgets? |
| RQ3 | How do task decomposability, tool-call count, and dependency depth moderate quality-throughput trade-offs? |
| RQ4 | Can quota-aware reservation improve goodput and deadline success without materially reducing task quality? |
| RQ5 | Does uncontrolled agent fan-out disadvantage smaller or lower-priority tenants? |

The hypotheses are designed to be falsifiable. In particular, the protocol investigates whether multi-agent workflows can achieve higher isolated-task success while delivering lower deadline-adjusted goodput when shared inference quotas are constrained.

## Experimental Design

### Confirmatory Architectures

Four experimental conditions are specified.

| ID | Architecture | Description |
|---|---|---|
| S1 | Single-agent | One agent completes the task using the frozen model, tools, and workflow budget. |
| S2 | Independent best-of-three | Three independent attempts execute against isolated task states, with all attempts charged to a common workflow budget. |
| M2 | Planner-executor-verifier | A planner creates a bounded task graph, up to two workers execute independent tasks, and a verifier checks the final state. |
| M3 | Quota-aware M2 | The same planner-executor-verifier topology with atomic reservation of request, token, and concurrency capacity before worker dispatch. |

The most important contrast is M3 versus M2, which isolates the effect of quota-aware admission while keeping the underlying multi-agent topology unchanged.

### Workload Families

<strong>AppWorld — API and Tool-Use Tasks</strong>

AppWorld provides simulated applications, structured APIs, and multi-step tool-use tasks. The protocol proposes selecting at least 60 unique tasks, stratified by dependency depth, branch count, and tool complexity.

<strong>Terminal-Bench — Terminal and Software Tasks</strong>

Terminal-Bench provides executable terminal and software engineering tasks with outcome verification. The protocol requires a frozen release, task identifiers, environment image digests, and grader versions before confirmatory execution.

Task-level license verification and reproducibility checks are required before confirmatory execution.

### Workload Conditions

| Factor | Prespecified conditions |
|---|---|
| Concurrency | 1, 4, 8, 16 logical workflows |
| Arrival patterns | Steady open-loop and burst |
| Quota profiles | Low, medium, high |
| Execution seeds | 3 independent seeds |
| Workload families | AppWorld and Terminal-Bench |
| Architectures | S1, S2, M2, M3 |

### Shared Quota Profiles

The protocol defines three normalized emulator quota profiles relative to measured single-agent demand.

| Profile | Normalized capacity |
|---|---:|
| Low | 0.5× |
| Medium | 1.0× |
| High | 2.0× |

The shared quota model covers:

- Requests per minute (RPM).
- Tokens per minute (TPM).
- Maximum in-flight concurrent requests.

These are normalized emulator settings, not claims about any commercial provider's subscription or quota tier.

## System Design

AgentQuotaBench moves the unit of evaluation from isolated agent calls to complete logical workflows competing for shared inference resources.

### Shared-Capacity Architecture

~~~mermaid
flowchart TD
    A[Incoming User Workflows] --> B[Workload Driver]
    B --> C{Workflow Architecture}

    C --> S1[S1 Single Agent]
    C --> S2[S2 Best of Three]
    C --> M2[M2 Planner Executor Verifier]
    C --> M3[M3 Quota-Aware M2]

    S1 --> Q[Shared Quota Ledger]
    S2 --> Q
    M2 --> Q
    M3 --> R[Capacity Reservation]
    R --> Q

    Q --> L[RPM TPM and Concurrency Checks]
    L --> D[Worker Dispatch]
    D --> E[Execution and Tool Calls]
    E --> F[Usage Reconciliation]
    F --> G[Verification and Grading]
    G --> H[Workflow Events and Metrics]
~~~

### Quota-Aware Admission Control

M3 introduces a transparent admission-control intervention.

The proposed control flow is:

1. Estimate child-agent request count and token demand.
2. Atomically reserve request, token, and in-flight capacity.
3. Admit only workers with granted capacity reservations.
4. Queue, reduce, or sequentially execute work when capacity is insufficient.
5. Reconcile estimated and actual resource usage.
6. Record fallback decisions, reservation errors, and overrun events.

The maximum worker count is two per workflow, with no recursive spawning beyond the fixed plan.

### Resource Accounting

Every model request, planner call, worker call, verifier call, failed attempt, and retry is included in resource accounting.

The benchmark records:

- Input, cached input, and output tokens.
- Estimated and reserved tokens.
- Actual usage and cost.
- Admission outcomes.
- Retry and failure events.
- Queue waiting time.
- Workflow completion and deadline status.

## Evaluation Metrics

The primary endpoint is deadline-adjusted goodput, with task success and p95 end-to-end latency serving as prespecified guardrails.

| Metric | Operational definition | Direction |
|---|---|---|
| Deadline-adjusted goodput | Successful on-time workflows per fixed measurement-window minute | Higher |
| Task success | Grader-passing workflows / all submitted workflows | Higher |
| Deadline success | Successful on-time workflows / all submitted workflows | Higher |
| p95 latency | 95th percentile end-to-end workflow latency | Lower |
| Queue delay | Waiting time for quota admission | Lower |
| Rate-limit rate | 429 responses / attempted requests | Lower |
| Retry amplification | All attempted requests / first-attempt requests | Lower |
| Token use | Total tokens across agents, failures, and retries | Lower, quality-aware |
| Cost per success | Total cost / successful workflows | Lower |
| Coordination overhead | Coordination tokens / total workflow tokens | Lower, quality-aware |
| Tenant fairness | Jain's index over normalized tenant service rates | Higher |
| Budget violations | Workflows exceeding configured resource ceilings | Lower |

### Tenant Fairness

The protocol uses Jain's fairness index to evaluate normalized tenant-level on-time service rates.

\[
J(x_1,\ldots,x_n)=
\frac{(\sum_{i=1}^{n}x_i)^2}
{n\sum_{i=1}^{n}x_i^2}
\]

The fairness analysis also reports absolute goodput, individual tenant completion rates, worst-tenant service, and starvation events.

Fairness is not interpreted independently of absolute service quality.

## Proposed Experimental Scale

The core confirmatory factorial specifies:

~~~text
60 tasks
× 2 workload families
× 4 architectures
× 4 concurrency levels
× 2 arrival profiles
× 3 quota profiles
× 3 independent seeds

= 34,560 workflow episodes
~~~

The separate mixed-tenant fairness track uses the same task pools and run seeds. Its episodes are counted separately.

A 24-task pilot study is specified before the confirmatory experiment to validate instrumentation, deterministic replay, resource accounting, and sample-size precision assumptions.

## Statistical Analysis

The statistical analysis plan specifies:

- Mixed-effects logistic regression for task success and deadline outcomes.
- Paired goodput differences and ratios with hierarchical bootstrap confidence intervals.
- Paired bootstrap intervals for latency quantiles.
- Tenant-level fairness and starvation analysis.
- Holm correction for confirmatory secondary pairwise comparisons.
- Absolute effect sizes, relative changes, confidence intervals, and sample counts.

The primary estimand is the paired difference in deadline-adjusted goodput between M3 and M2 across confirmatory shared-quota conditions.

For task success, the proposed M3 versus M2 non-inferiority margin is three percentage points. This is a design tolerance, not an observed result.

## Reproducibility and Validation

The protocol specifies a reproducibility package containing:

- Versioned task manifests and task identifiers.
- Dataset releases, environment digests, and grader versions.
- Frozen model/provider configurations.
- Quota profiles and workload arrival schedules.
- Prompt and tool configurations.
- Deterministic fault-injection schedules.
- JSONL event traces.
- Run seeds and workflow-level outcomes.
- Statistical analysis scripts and result tables.

The source protocol reports nine passing deterministic software tests for a small Python reference model. These cover token-bucket refill, atomic reservations, concurrency limits, reconciliation, lease expiry, retry admission, and deterministic quota accounting.

These tests validate selected reference-model mechanics only. They do not establish production quota-governor correctness, real-provider behavior, benchmark performance, or research hypotheses.

## Protocol Status

| Component | Status |
|---|---|
| Research protocol narrative | Prepared |
| Four confirmatory architectures | Specified |
| Workload families | Selected; exact task releases and licenses pending |
| Shared quota model | Specified |
| Admission-control intervention | Specified |
| Statistical analysis plan | Specified |
| Reference-model software tests | 9 reported as passing |
| Public preregistration | Pending |
| Full benchmark harness | Pending |
| Pilot execution | Pending |
| Confirmatory experiments | Pending |
| Empirical results | Not available |
| Public benchmark code and run artifacts | Pending |

<strong>Publication integrity:</strong> This repository describes a research protocol, not a completed empirical benchmark. No benchmark task-success rates, goodput measurements, latency quantiles, p-values, confidence intervals, or cost-per-success findings are reported.

The full benchmark harness and confirmatory results must be made available separately as they are implemented and validated.

## Citation

Please cite the protocol as follows:

Ashraf, M. A. (2026). <em>AgentQuotaBench: Evaluating Multi-Agent LLM Workflows Under Shared Inference Quotas</em>. Research Protocol, Version 1.1. Zenodo.

### BibTeX

{% raw %}
~~~bibtex
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
~~~
{% endraw %}

<strong>DOI:</strong> [10.5281/zenodo.24279037](https://doi.org/10.5281/zenodo.24279037)  
<strong>Zenodo record:</strong> [https://zenodo.org](https://zenodo.org)

## Author

<div align="center">

<strong>Muhammad Attayab Ashraf</strong>

Independent Researcher | Founder & CEO, Automivex

Lahore, Pakistan

[![GitHub](https://img.shields.io/badge/GitHub-Attayab--31-181717?style=flat-square&logo=github)](https://github.com/Attayab-31)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Muhammad%20Attayab-0A66C2?style=flat-square&logo=linkedin)](https://www.linkedin.com/in/muhammad-attayab-ashraf-519562262/)
[![Website](https://img.shields.io/badge/Website-Attayab-4A90E2?style=flat-square&logo=googlechrome&logoColor=white)](https://attayab.automivex.com)
[![Email](https://img.shields.io/badge/Email-Contact-EA4335?style=flat-square&logo=gmail&logoColor=white)](mailto:attayabpc2@gmail.com)

</div>

## Contact and Research Collaboration

For academic inquiries, research discussions, collaboration opportunities, or questions regarding AgentQuotaBench, contact:

- <strong>Author:</strong> Muhammad Attayab Ashraf
- <strong>Email:</strong> [attayabpc2@gmail.com](mailto:attayabpc2@gmail.com)
- <strong>GitHub:</strong> [github.com/Attayab-31](https://github.com/Attayab-31)
- <strong>Portfolio:</strong> [attayab.automivex.com](https://attayab.automivex.com)
- <strong>LinkedIn:</strong> [Muhammad Attayab Ashraf](https://www.linkedin.com/in/muhammad-attayab-ashraf-519562262/)

---

<em>AgentQuotaBench aims to contribute to the evaluation of reliable, capacity-aware, and resource-efficient multi-agent LLM systems through controlled experiments and reproducible benchmark methodology.</em>

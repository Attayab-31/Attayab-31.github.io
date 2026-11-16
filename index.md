---
layout: default
title: Muhammad Attayab Ashraf | AI Systems Research
description: Research portfolio focused on multi-agent LLM systems, inference serving, capacity-aware scheduling, and reproducible AI research.
lang: en
---

<div class="page-header" style="text-align: center;">
  <h1>Muhammad Attayab Ashraf</h1>
  <p><strong>Independent Researcher | AI Systems | Multi-Agent LLMs | Agentic AI</strong></p>
  <p>Research on reliable, capacity-aware, and reproducible AI systems, with a focus on how multi-agent workflows behave under shared inference constraints.</p>
  <p>
    <a href="https://github.com/Attayab-31">GitHub</a> ·
    <a href="https://www.linkedin.com/in/muhammad-attayab-ashraf-519562262/">LinkedIn</a> ·
    <a href="https://attayab.automivex.com">Professional Website</a> ·
    <a href="mailto:attayabpc2@gmail.com">Email</a>
  </p>
</div>

<hr>

<h2 id="about">About</h2>
<p>I am an independent researcher and the Founder and CEO of <strong>Automivex</strong>, based in Lahore, Pakistan. My research interests center on AI systems engineering, multi-agent LLM workflows, inference-serving infrastructures, resource allocation, and reproducible benchmark design.</p>
<p>My work explores the systems questions that emerge when capable AI agents move from isolated experiments into concurrent, resource-constrained applications. I am particularly interested in evaluating how multi-agent workflows perform under shared inference quotas, admission control, queueing pressure, and fairness constraints.</p>

<h2 id="research-interests">Research Interests</h2>
<ul>
  <li>Multi-Agent Systems and Agentic AI</li>
  <li>Large Language Model Systems and Workflow Orchestration</li>
  <li>Inference Serving, Quota Management, and Resource Allocation</li>
  <li>Capacity-Aware Scheduling and Admission Control</li>
  <li>Queueing, Tail Latency, and Service-Level Goodput</li>
  <li>Multi-Tenant Fairness in Shared AI Infrastructure</li>
  <li>Benchmark Design, Statistical Evaluation, and Reproducible Research</li>
</ul>

<h2 id="featured-research">Featured Research</h2>
<h3>AgentQuotaBench</h3>
<p><strong>Evaluating Multi-Agent LLM Workflows Under Shared Inference Quotas</strong></p>
<p><em>A reproducible benchmark protocol for capacity-aware admission control, deadline-adjusted goodput, and tenant fairness.</em></p>
<p><strong>Research Protocol | Version 1.1 | Published November 16, 2026</strong></p>
<p><a href="https://doi.org/10.5281/zenodo.24279037">Read the protocol record via DOI</a> · <a href="https://zenodo.org">Zenodo</a></p>

<h3>Research overview</h3>
<p>AgentQuotaBench investigates how multi-agent large language model workflows behave when concurrent user workflows compete for a shared inference-capacity pool. Multi-agent architectures can improve task completion by parallelizing research, planning, execution, and verification, but the same fan-out concentrates demand on shared inference resources.</p>
<p>The protocol proposes a controlled comparison of four workflow architectures under matched task and resource-accounting rules. It evaluates whether additional agent parallelism can improve isolated-task outcomes without reducing service-level goodput, deadline attainment, or tenant fairness under a fixed quota model.</p>

<h3>Abstract</h3>
<p>Multi-agent large language model workflows can improve task completion by parallelizing research, planning, execution, and verification. The same fan-out increases demand on shared inference capacity, creating bottlenecks in quota-limited settings. AgentQuotaBench studies this trade-off through a benchmark protocol designed to compare workflow architectures under a common shared inference quota and to evaluate how quota-aware control changes performance, latency, and fairness.</p>

<h3>Research question</h3>
<blockquote>When many user workflows share a finite inference quota, which workflow architecture delivers the most correct, on-time completions per unit of shared capacity, and can quota-aware admission improve service-level goodput without degrading task quality?</blockquote>

<h3>Experimental conditions</h3>
<table>
  <thead><tr><th>ID</th><th>Workflow</th><th>Experimental role</th></tr></thead>
  <tbody>
    <tr><td>S1</td><td>Single-agent</td><td>Baseline: one agent completes the task.</td></tr>
    <tr><td>S2</td><td>Independent best-of-three</td><td>Three independent attempts run on isolated task states; all attempts and aggregation are charged to the same total per-workflow ceiling.</td></tr>
    <tr><td>M2</td><td>Planner-executor-verifier</td><td>Unconstrained multi-agent baseline: a planner creates a bounded task graph, up to two workers execute independent leaves, and a verifier checks the final state.</td></tr>
    <tr><td>M3</td><td>Quota-aware M2</td><td>Same topology as M2, with atomic reservation of request, token, and in-flight capacity before worker dispatch; may wait, reduce worker count, or fall back to sequential execution when the shared quota is insufficient.</td></tr>
  </tbody>
</table>

<h3>Workloads and load conditions</h3>
<table>
  <thead><tr><th>Dimension</th><th>Prespecified design</th></tr></thead>
  <tbody>
    <tr><td>Workload families</td><td>AppWorld API and tool-use tasks, and a frozen Terminal-Bench release</td></tr>
    <tr><td>Task selection</td><td>At least 60 unique tasks per workload family, subject to task-level licensing and reproducibility checks</td></tr>
    <tr><td>Concurrency</td><td>1, 4, 8, and 16 logical workflows</td></tr>
    <tr><td>Arrival patterns</td><td>Steady open-loop and burst</td></tr>
    <tr><td>Quota profiles</td><td>Low (0.5 times), medium (1.0 times), and high (2.0 times) normalized reference capacity</td></tr>
    <tr><td>Replication</td><td>Three independent execution seeds</td></tr>
  </tbody>
</table>
<p>The quota multipliers are emulator settings normalized to measured single-agent demand. They are not claims about any provider's commercial plan or published quota tier.</p>

<h3>Primary endpoint and evaluation metrics</h3>
<p>The single primary endpoint is <strong>deadline-adjusted goodput</strong>: grader-passing workflows completed before their frozen deadlines per minute of a fixed open-loop measurement window. Workflows submitted after the deadline or failing validation do not count as successful goodput.</p>
<p>Prespecified guardrails are <strong>task success</strong> and <strong>p95 end-to-end latency</strong>. Secondary measures include queue delay, rate-limit responses, retry amplification, token use, cost per successful task, coordination overhead, and tenant fairness outcomes.</p>
<p>For the separate mixed-tenant fairness track, the protocol specifies Jain's fairness index alongside absolute goodput, per-tenant service rates, worst-tenant outcomes, and starvation events.</p>

<h3>Proposed experimental scale</h3>
<p style="text-align: center; font-size: 2em;"><strong>34,560</strong></p>
<p style="text-align: center;"><strong>Core confirmatory workflow episodes</strong></p>
<p style="text-align: center;">60 tasks × 2 workload families × 4 architectures × 4 concurrency levels × 2 arrival profiles × 3 quota profiles × 3 independent seeds</p>
<p>The mixed-tenant fairness track is additional and is counted separately from the core factorial design.</p>

<h2 id="research-principles">Research Principles</h2>
<p><strong>Controlled comparison.</strong> The four architectures are evaluated with matched task instructions, a pinned model and provider configuration, equivalent tools and state snapshots, explicit workflow budgets, and a shared quota ledger.</p>
<p><strong>Service-level evaluation.</strong> The protocol evaluates complete logical workflows rather than relying only on isolated request counts, token totals, or average latency. Goodput, deadline attainment, and fairness are treated as first-class outcome measures.</p>
<p><strong>Transparent intervention.</strong> M3 changes worker admission rather than the underlying model or communication representation. It reserves estimated shared capacity before dispatch and records reservation, fallback, and overrun decisions.</p>
<p><strong>Reproducibility.</strong> The proposed release package includes versioned task manifests, model and quota configurations, arrival and fault schedules, prompts, event schemas, run seeds, sanitized traces, and analysis scripts.</p>

<h2 id="protocol-status">Protocol Status</h2>
<p>This work is a <strong>research protocol and benchmark design</strong>, not a completed empirical evaluation. The manuscript specifies the research questions, confirmatory conditions, workload plan, quota model, and statistical analysis plan, but the full benchmark harness and data collection remain pending.</p>
<table>
  <thead><tr><th>Component</th><th>Status in protocol version 1.1</th></tr></thead>
  <tbody>
    <tr><td>Protocol narrative and hypotheses</td><td>Specified</td></tr>
    <tr><td>Four confirmatory architectures</td><td>Specified</td></tr>
    <tr><td>AppWorld and Terminal-Bench workload plan</td><td>Specified; exact task releases, task IDs, and component licenses remain to be frozen and checked</td></tr>
    <tr><td>Shared quota model and normalized profiles</td><td>Specified; exact emulator settings to be frozen after the pilot</td></tr>
    <tr><td>Quota-aware M3 intervention</td><td>Specified</td></tr>
    <tr><td>Statistical analysis plan</td><td>Specified; pilot-based precision analysis remains required</td></tr>
    <tr><td>Deterministic quota reference-model tests</td><td>Nine software tests reported as passing</td></tr>
    <tr><td>Public timestamped preregistration</td><td>Pending in the manuscript's submission-readiness checklist</td></tr>
    <tr><td>Full benchmark harness and provider adapter</td><td>Pending</td></tr>
    <tr><td>Pilot execution</td><td>Pending</td></tr>
    <tr><td>Confirmatory benchmark execution</td><td>Pending</td></tr>
    <tr><td>Empirical findings and benchmark result tables</td><td>Not available</td></tr>
    <tr><td>Full public code and run-artifact release</td><td>Pending</td></tr>
  </tbody>
</table>
<p>The nine reported tests cover selected reference-model mechanics only, including token-bucket refill, atomic reservation, concurrency limits, usage reconciliation, lease expiry, retry admission, and deterministic quota accounting. They do not establish benchmark performance or any final empirical claim.</p>
<p>No task-success rates, latency quantiles, goodput values, provider rate-limit rates, confidence intervals, p-values, or cost-per-success findings are claimed. Any future empirical claims must be supported by separate benchmark execution and publication.</p>

<h2 id="publication-details">Publication Details</h2>
<table>
  <tbody>
    <tr><th>Title</th><td>AgentQuotaBench: Evaluating Multi-Agent LLM Workflows Under Shared Inference Quotas (Research Protocol)</td></tr>
    <tr><th>Author</th><td>Muhammad Attayab Ashraf</td></tr>
    <tr><th>Affiliation</th><td>Independent Researcher; Founder and CEO, Automivex</td></tr>
    <tr><th>Protocol version</th><td>1.1</td></tr>
    <tr><th>Publication date</th><td>November 16, 2026</td></tr>
    <tr><th>Archive and publisher</th><td>Zenodo</td></tr>
    <tr><th>DOI</th><td><a href="https://doi.org/10.5281/zenodo.24279037">10.5281/zenodo.24279037</a></td></tr>
    <tr><th>Canonical DOI resolver</th><td><a href="https://doi.org">https://doi.org</a></td></tr>
    <tr><th>Zenodo record</th><td><a href="https://zenodo.org">https://zenodo.org</a></td></tr>
    <tr><th>Asset status</th><td>PDF-only protocol on Zenodo. No duplicate PDF is hosted on GitHub.</td></tr>
  </tbody>
</table>

<h2 id="citation">Citation</h2>
<p>Ashraf, M. A. (2026). <em>AgentQuotaBench: Evaluating Multi-Agent LLM Workflows Under Shared Inference Quotas</em> (Research Protocol, Version 1.1). Zenodo. <a href="https://doi.org/10.5281/zenodo.24279037">https://doi.org/10.5281/zenodo.24279037</a></p>
<h3>BibTeX</h3>
<pre><code>@misc{ashraf2026agentquotabench,
  author       = {Ashraf, Muhammad Attayab},
  title        = {AgentQuotaBench: Evaluating Multi-Agent LLM Workflows Under Shared Inference Quotas},
  year         = {2026},
  month        = nov,
  doi          = {10.5281/zenodo.24279037},
  url          = {https://zenodo.org},
  howpublished = {Zenodo},
  note         = {Research Protocol, Version 1.1. Published November 16, 2026. Confirmatory experiments pending.}
}</code></pre>

<h2 id="author">Author</h2>
<p><strong>Muhammad Attayab Ashraf</strong><br>
Independent Researcher | Founder and CEO, Automivex<br>
Lahore, Pakistan</p>
<p><a href="https://github.com/Attayab-31">GitHub</a> · <a href="https://www.linkedin.com/in/muhammad-attayab-ashraf-519562262/">LinkedIn</a> · <a href="https://attayab.automivex.com">Website</a> · <a href="mailto:attayabpc2@gmail.com">Email</a></p>
<p>For academic inquiries, research discussion, or potential collaboration, contact <a href="mailto:attayabpc2@gmail.com">attayabpc2@gmail.com</a>.</p>

<hr>
<p style="text-align: center;"><em>Building and evaluating reliable AI systems through rigorous methodology, transparent reporting, and reproducible research.</em></p>

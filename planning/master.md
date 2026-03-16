# Google Antigravity — Implementation Plan
### Agentic Financial Compliance Platform | v1.0 | Q2 2026

---

## Table of Contents
1. [Executive Summary](#1-executive-summary)
2. [Product Vision & Strategy](#2-product-vision--strategy)
3. [Multi-Agent Architecture](#3-multi-agent-architecture)
4. [The Four-Layer Stack](#4-the-four-layer-stack)
5. [Technical Architecture](#5-technical-architecture)
6. [Regulatory & Compliance Map](#6-regulatory--compliance-map)
7. [Team Structure](#7-team-structure)
8. [Phased Roadmap](#8-phased-roadmap)
9. [90-Day Pilot Playbook](#9-90-day-pilot-playbook)
10. [Risks & Mitigations](#10-risks--mitigations)
11. [Success Metrics & KPIs](#11-success-metrics--kpis)

---

## 1. Executive Summary

Google Antigravity is a compliance-by-design agentic platform for financial services. It orchestrates a squad of AI agents across KYC onboarding, AML transaction monitoring, fraud detection, case investigation, and regulatory reporting — with every decision traceable, explainable, and audit-ready.

**The core problem it solves:** AML alert queues have a 90–95% false-positive rate industry-wide. Compliance teams spend the vast majority of their time reviewing noise instead of investigating real financial crime. Antigravity reduces that false-positive rate to under 20% and cuts per-case investigation time from ~50 minutes to under 8 minutes.

### Key Numbers

| Metric | Industry Baseline | Antigravity Target |
|---|---|---|
| AML false positive rate | 90–95% | < 20% |
| KYC onboarding (straight-through) | ~60% | > 90% |
| Time per L1 case investigation | 45–60 min | < 8 min |
| Periodic KYC review cycle | 3–5 years | Event-driven / continuous |
| ROI timeline | — | 2.3× in 13 months (IDC benchmark) |

### Pilot Commitment
Deploy the KYC Onboarding Agent and Transaction Monitoring Agent at one mid-size US bank ($10B–$50B assets) within **90 days**. Human approval required for all consequential decisions from day one.

---

## 2. Product Vision & Strategy

### Vision Statement
> Make compliance a competitive advantage — not a cost center. Every bank that runs Google Antigravity will process more customers, catch more crime, and face fewer regulatory fines than any competitor still managing alert queues manually.

### What Makes This Different

| Point Solutions (old world) | Google Antigravity (new world) |
|---|---|
| Manual alert review queues | Autonomous L1/L2 case resolution; human sign-off on exceptions only |
| Periodic KYC reviews (3–5 years) | Perpetual KYC: event-driven, continuous risk assessment |
| Siloed KYC, AML, fraud tools | Unified intelligence: shared entity graph, cross-product signals |
| "The model said so" explanations | Full cited reasoning chain per decision — regulator-ready |
| One-size-fits-all detection rules | Per-institution, per-customer personalized risk models |

### Target Segments

**Primary — Tier 2/3 US Banks ($5B–$100B assets)**
These institutions carry large-bank regulatory burden without large-bank technology budgets. They are the most underserved, most motivated buyers and cannot build this themselves. 400+ eligible institutions in the US alone. Primary buyer: CCO + CTO.

**Secondary — Fintechs scaling compliance**
Growing fintechs that have outgrown point solutions and face their first regulatory examination. They want to build compliance infrastructure once, correctly, and not revisit it for 5 years.

**Tertiary — Insurance & Asset Management (Year 2)**
Similar NAIC/SEC obligations, similar alert fatigue, less mature vendor ecosystem.

### Competitive Moats

| Layer | What it is | Why it compounds |
|---|---|---|
| Compliance RAG | Proprietary indexes: live sanctions, regulatory text, adverse news, prior SAR corpus | Gets smarter with each institution's confirmed cases added to the eval set |
| Audit infrastructure | Immutable regulator-readable decision logs from day one | Switching cost: regulators see 2+ years of clean audit history |
| Eval benchmarks | Domain-specific ground truth: KYC accuracy, FP rate, SAR precision | Published benchmarks become a sales tool — competitors can't replicate without the case data |
| Regulatory intelligence | Auto-ingests FinCEN/FCA/AMLA/AUSTRAC rulemaking feeds | Every new regulation reinforces retention — we update the rules, they don't have to |

---

## 3. Multi-Agent Architecture

### Architecture Principles
- **Minimal footprint** — each agent does one thing extremely well and delegates everything else
- **Declared capabilities** — every agent publishes a capability manifest; it cannot call APIs outside that manifest
- **No autonomous consequential actions** — all fund movements, SAR filings, and account freezes require human approval tokens
- **Full lineage** — every prompt, retrieved chunk, tool call, and output is logged with a trace ID
- **Graceful degradation** — if a specialist agent fails, the orchestrator routes to human queue; never silently drops a case

### Agent Roster

```
┌─────────────────────────────────────────────────────────────┐
│                    ORCHESTRATOR AGENT                        │
│  Plans · Delegates · Resolves conflicts · Maintains audit   │
└────────┬──────────────┬──────────────────┬──────────────────┘
         │              │                  │
    ┌────▼─────┐  ┌─────▼──────┐   ┌──────▼──────┐
    │  INTAKE  │  │  MONITOR   │   │ INVESTIGATE │
    │ Agents   │  │  Agents    │   │  Agents     │
    └────┬─────┘  └─────┬──────┘   └──────┬──────┘
         │              │                  │
  ┌──────┴───────┐  ┌───┴──────────┐  ┌───┴──────────────┐
  │• KYC Onboard │  │• Txn Monitor │  │• Case Investigator│
  │• Identity V. │  │• Fraud Detect│  │• SAR/STR Filing  │
  │• Benef. Owner│  │• Perp. KYC  │  │• QA Validator    │
  └──────────────┘  │• Sanctions   │  │• Reg Reporting   │
                    └──────────────┘  └──────────────────┘
```

### Agent Specifications

| Agent | Tier | Primary Function | Key Guardrail |
|---|---|---|---|
| **Orchestrator** | Supervisor | Plans, delegates, resolves conflicts, maintains global audit log | Cannot execute actions directly — routes only |
| **KYC Onboarding** | Intake | Document verification, PEP/sanctions screening, risk tier assignment | Human approval for high-risk tier assignments |
| **Identity Verification** | Intake | Deepfake detection, liveness checks, synthetic identity scoring | Sub-second scoring; flags to human queue if confidence < 85% |
| **Beneficial Owner** | Intake | UBO mapping through corporate hierarchies, CTA compliance | Cannot designate UBO without source citation |
| **Transaction Monitor** | Monitor | Real-time behavioral pattern detection across all payment rails | Alert escalation within 60 seconds; no autonomous action |
| **Fraud Detection** | Monitor | Multi-vector fraud: ATO, synthetic identity, payment fraud | Sub-100ms scoring; hard block requires human confirmation |
| **Perpetual KYC** | Monitor | Event-driven continuous risk reassessment | Re-review triggered automatically; outcome requires analyst sign-off |
| **Sanctions Screening** | Monitor | Millisecond OFAC/EU/UN screening with fuzzy name matching | Zero false-negative tolerance; unresolved match auto-escalates |
| **Case Investigator** | Investigate | Autonomous L1/L2 case handling: evidence gathering + narrative | Drafts only; human analyst approves all case closures |
| **SAR/STR Filing** | Investigate | Draft SAR in jurisdiction-specific format, route for sign-off | **HARD STOP:** Cannot file without two-person integrity approval |
| **QA Validator** | Investigate | Reviews all agent outputs for accuracy, bias, hallucination | Runs on every output before human delivery; failures re-queue |
| **Reg Reporting** | Investigate | Aggregates compliance metrics, generates CTR/MIS submissions | Draft only; CCO approval required before submission |

### Orchestration Loop (Plan–Execute–Verify)
1. Receive trigger (new customer, transaction alert, adverse news event, scheduled review)
2. Classify case type and required agent squad
3. Dispatch specialist agents in parallel where possible (KYC + Beneficial Owner run concurrently)
4. Aggregate outputs into unified case state object
5. Route to QA Validator for accuracy and bias checks
6. Deliver to human queue with full evidence bundle and recommended action
7. Capture human decision as feedback signal for continuous improvement

### Sample Capability Manifest — KYC Onboarding Agent
```json
{
  "agent": "kyc-onboarding-v1",
  "allowed_tools": [
    "GET /sanctions/screen",
    "GET /adverse-news/search",
    "GET /corporate-registry/lookup",
    "GET /entity/risk-profile",
    "POST /case/create",
    "POST /audit-log/append"
  ],
  "explicitly_denied": [
    "POST /account/freeze",
    "POST /sar/file",
    "POST /transaction/block",
    "POST /customer/notify",
    "ANY /cross-institution/*"
  ],
  "max_tokens_per_call": 4000,
  "requires_human_approval": ["risk_tier:HIGH", "risk_tier:VERY_HIGH"]
}
```

---

## 4. The Four-Layer Stack

### Layer 1: RAG — Knowledge & Retrieval

The knowledge layer is what separates a generic LLM agent from a compliance-grade product.

**Data Sources**

| Source | Update Frequency | Notes |
|---|---|---|
| OFAC SDN List | Every 4 hours | Zero-tolerance freshness — stale = regulatory violation |
| EU/UN Consolidated Sanctions | Every 4 hours | Separate index for jurisdiction-aware queries |
| Adverse News (Dow Jones, LexisNexis) | Every 15 minutes | Semantic search + entity disambiguation by resolved ID |
| Corporate Registries (EDGAR, Companies House, OpenCorporates) | Daily | Force-refresh on ownership change event |
| FinCEN / FCA / AMLA Regulatory Text | On publish event | Subscribe to rulemaking RSS; re-index + trigger regression eval |
| Core Banking / Transaction History | Real-time stream | Vector-index behavioral baselines; never store raw PII in vector DB |
| Prior Investigation Memos & SARs | On case close | Internal per-institution corpus; fine-tunes institution-specific style |
| PEP Databases (World-Check, Refinitiv) | Daily | Dedicated high-precision index, separate from standard adverse news |

**Retrieval Design**
- **Hybrid retrieval:** dense semantic (embedding-based) + BM25 sparse. Regulatory text needs both exact citation matching AND conceptual similarity for novel scenarios
- **Risk-weighted re-ranking:** surfaces SDN hits above name similarities regardless of raw relevance score
- **Multi-tenant isolation:** hard namespace separation per institution; separate vector collections with institution-scoped API keys; no cross-institution retrieval under any circumstance
- **Citation anchoring (non-negotiable):** every agent output must cite exact retrieved chunks — source name, entry ID, timestamp retrieved, and confidence score. "Source: OFAC SDN List" is insufficient

**RAG vs Fine-Tune Decision**
- RAG: sanctions lists, regulatory docs, adverse news, case law — change constantly
- Fine-tune: institution-specific memo format, risk taxonomy, escalation phrasing — change rarely
- Never fine-tune on customer PII

---

### Layer 2: Evals — Quality Benchmarks

Evals are Antigravity's most defensible moat. A competitor can copy the agent architecture. They cannot copy two years of confirmed financial crime cases used to calibrate ground truth.

**Eval Hierarchy**

| Level | What it Tests | Cadence |
|---|---|---|
| L0 — Unit | Individual agent tool calls and output schema validation | Every commit in CI/CD; blocks merge on failure |
| L1 — Behavioral | Agent decision quality on golden dataset of confirmed cases | Every deploy; blocks release if regression > 0.5% |
| L2 — Human-in-Loop | Expert analyst review of 100 sampled outputs per week | Weekly; feeds back into L1 golden dataset |
| L3 — Adversarial | Red-team: prompt injection, data poisoning, edge cases | Monthly + after any regulatory change event |
| L4 — Regulatory | Simulate regulator examination: reconstruct decisions from audit log | Quarterly; required for SOC 2 and model risk attestation |

**Key Metrics by Agent**

| Agent | Primary Metric | Target | Industry Baseline |
|---|---|---|---|
| KYC Onboarding | Straight-through processing rate | > 90% | ~60% |
| Transaction Monitor | False positive rate | < 20% | 90–95% |
| Fraud Detection | Precision/Recall F1 | > 0.92 | ~0.71 (rule-based) |
| Case Investigator | Investigation time (L1) | < 8 min | 45–60 min |
| SAR/STR Filing | Citation faithfulness | < 1% hallucinated citations | N/A (manual today) |
| All Agents | Disparate impact ratio | > 0.80 (EU AI Act threshold) | Untested at most institutions |
| All Agents | Latency p99 | Sanctions: < 100ms; KYC: < 3 min; SAR draft: < 8 min | N/A |

**Golden Dataset Construction**
Three required datasets — refresh quarterly:
1. **Confirmed positives:** cases that resulted in SARs or enforcement actions
2. **Confirmed negatives:** cases reviewed and cleared by human analysts
3. **Adversarial cases:** synthetic scenarios probing specific failure modes (name fuzzing, layered ownership structures, synthetic identities)

---

### Layer 3: Guardrails — Compliance-by-Design

> **Design philosophy:** Guardrails are not a compliance checkbox bolted on at the end. They are the architecture. Build them first.

**Hard Stops — Structurally Impossible Actions**

| Action | Why it's a hard stop | Technical Enforcement |
|---|---|---|
| Autonomous fund transfer or debit | Irreversible; direct customer harm | Cryptographic approval token required in action payload; absent = API 403 |
| Autonomous SAR/STR filing | Legal act with regulatory consequences; tipping-off risk | Two-person integrity: CCO + MLRO digital signatures required |
| Account freeze or closure | Customer impact; due process requirements | Flag only; 4-eyes approval within institution-defined SLA |
| Customer-facing comms about investigation | Tipping-off offense in most jurisdictions | Output classifier blocks any draft mentioning investigation to customer |
| Cross-institution data retrieval | GDPR / CCPA / GLBA violation | Namespace isolation at infrastructure level; not configurable |

**Soft Guardrails — Human-in-the-Loop Triggers**
- Model confidence < 85% on any risk tier assignment → auto-escalate to analyst queue
- Novel scenario not covered by training distribution → flag and route, never guess
- Contradictory signals (clean history + adverse news hit) → dual-agent review
- High-value customer relationship flag → CCO notification before any adverse action
- New jurisdiction or regulatory change event → pause affected workflow, notify compliance team

**Audit Log Schema (Immutable)**
```json
{
  "trace_id": "uuid-v4 — links all sub-events to one case action",
  "timestamp_utc": "ISO 8601 with ms precision — immutable once written",
  "agent_id": "agent name + deployed version — enables reproducibility",
  "input_hash": "SHA-256 of prompt + context window — proves exact input",
  "retrieved_chunks": "[{source, chunk_id, timestamp_retrieved, confidence}]",
  "tool_calls": "[{tool_name, parameters, response, latency_ms}]",
  "output": "agent decision/recommendation",
  "output_hash": "SHA-256 of output — proves log was not modified",
  "human_decision": "{analyst_id, decision, timestamp, justification} if escalated"
}
```

---

### Layer 4: Personalization — Adaptation at Every Level

**Three Levels**

| Level | What it Adapts | How it Works |
|---|---|---|
| Institution | Risk taxonomy, memo format, escalation thresholds, jurisdiction list | Config namespace injected into every agent system prompt at invocation |
| Analyst | Verbosity, evidence depth, recommendation confidence display | Role-based tiers (Director → New Hire); set per user in IAM profile |
| Entity | Per-customer behavioral baseline, relationship context, network position, geopolitical exposure | Living risk profile updated continuously via Perpetual KYC agent |

**Memory Architecture**

| Memory Type | Scope | Storage |
|---|---|---|
| Short-term (investigation context) | Current case session | Structured state object in Firestore; shared across agents on same case |
| Long-term (entity graph) | Persistent per customer | Neo4j: behavioral baselines, prior alerts, network relationships |
| Preference memory (analyst feedback) | Per analyst + per institution | Correction signals stored → folded into next eval cycle |
| Regulatory memory (live rule changes) | Global + jurisdiction-specific | Auto-updated on FinCEN/FCA/AMLA/AUSTRAC publish events; triggers regression eval |

**The Feedback Flywheel:**
Analyst overrides → stored as corrections → reviewed in L2 evals → folded into L1 golden dataset → model improves → analyst overrides decrease → product gets stickier per institution over time.

---

## 5. Technical Architecture

### Stack Decisions

| Layer | Technology | Rationale |
|---|---|---|
| LLM Foundation | Gemini 1.5 Pro + Claude Sonnet (ensemble) | Gemini for speed/cost on standard cases; Claude for complex reasoning and citation-heavy SARs |
| Orchestration | Google Cloud Workflows + custom agent router | Declarative step definitions enable audit trail at workflow level, not just LLM level |
| Vector Database | Vertex AI Vector Search (multi-tenant) | Native GCP integration; hard namespace isolation; 99.9% SLA on retrieval latency |
| Knowledge Indexing | Vertex AI Search + custom ETL pipelines | Handles PDF, SQL, real-time streams; hybrid BM25 + dense retrieval built-in |
| Audit Store | BigQuery (append-only partitioned tables) | Immutable event log at scale; SQL-queryable for regulator examinations; 7-year retention |
| Entity Graph | Neo4j Aura Enterprise | Relationship traversal for ownership chains; 4-hop traversal in < 200ms |
| Agent State | Firestore (per-case state machine) | Durable state for long-running cases; survives agent restarts; enables agent handoff |
| API Gateway | Apigee + capability manifest enforcement | All agent tool calls route through Apigee; manifest enforcement at gateway — rejected calls never reach target API |
| Eval Pipeline | Vertex AI Pipelines + custom eval harness | Automated L0/L1 on every deploy; LLM-as-judge for L2; results in BigQuery for trends |
| Observability | Cloud Trace + Cloud Logging + Grafana | Per-trace latency; alert on p99 > SLA; per-institution compliance KPI dashboards |
| Encryption | Cloud KMS with CMEK per institution | Customer-managed encryption keys; institution can revoke access to their data at any time |
| Identity & Access | Cloud IAM + Workforce Identity Federation | Roles: Analyst, Senior Analyst, CCO, MLRO, Auditor; federated with institution's IdP |

### Data Flow (transaction monitoring example)
```
Transaction Event (Kafka/Pub-Sub)
    │
    ▼
API Gateway (Apigee) — capability manifest check
    │
    ▼
Orchestrator Agent — classifies as AML monitoring case
    ├──► Transaction Monitor Agent — behavioral analysis
    ├──► Sanctions Screening Agent — name/entity check (parallel)
    └──► Fraud Detection Agent — fraud vector scoring (parallel)
         │
         ▼
    QA Validator Agent — accuracy, bias, hallucination check
         │
         ▼
    Audit Log (BigQuery append-only)
         │
         ▼
    Human Queue (case severity-ranked) or Auto-close (< risk threshold)
         │
         ▼
    Analyst Decision → Feedback Signal → Eval Pipeline
```

### Security Architecture
- **Data residency:** institution data never leaves its designated GCP region
- **Network isolation:** each institution deployment in separate VPC with private service connect
- **Key management:** CMEK per institution; key rotation every 90 days; audit log of all key usage
- **Penetration testing:** quarterly red-team; scope includes agent prompt injection attacks
- **Model supply chain:** hash-pinned model versions; no silent model updates in production

---

## 6. Regulatory & Compliance Map

### 2026 Regulatory Landscape

| Regulation | Jurisdiction | Status | What Antigravity Must Address |
|---|---|---|---|
| **FinCEN NPRM** | US | Active 2024–2026 | AI-focused risk assessments; explainable decisions; "the model said so" is not a valid defense |
| **EU AMLA Regulation** | EU | Finalizing 2026 | Harmonized standards across EU member states; jurisdiction-aware rule injection per entity domicile |
| **NACHA ACH Fraud Monitoring** | US | Mandatory 2026 | All ACH participants must document fraud monitoring; agent logs are evidence of compliance |
| **NYDFS Cybersecurity Guidance** | NY, US | Active Oct 2024 | AI risks classified as cybersecurity risks; agents must be in scope for pentesting and incident response |
| **EU AI Act — High-Risk Classification** | EU | Active | Credit scoring and fraud detection = high-risk AI; conformity assessments + human oversight mandated |
| **AUSTRAC Tranche 2** | Australia | Live 2026 | Extends AML to "gatekeeper" professions; KYC agents must handle lawyers, accountants, real estate |
| **SR 11-7 (Model Risk Management)** | US (Fed/OCC) | Ongoing | Model validation framework; independent validation of all models used in regulated decisions |
| **GDPR / CCPA / GLBA** | EU / CA / US | Ongoing | Right to explanation; data minimization; cross-border data transfer restrictions |

### Compliance Architecture Decisions Driven by Regulation

**FinCEN NPRM → Citation anchoring**
Every flagged or cleared customer decision requires a documented reasoning chain. Antigravity's audit log schema is built to satisfy this requirement on every case, automatically.

**EU AI Act → Bias testing as a first-class eval**
Fraud detection and credit scoring are classified high-risk. Disparate impact testing across demographic proxies is a mandatory eval metric, not optional.

**NACHA 2026 → Agent logs as compliance evidence**
Antigravity's append-only BigQuery audit log serves as the documented fraud monitoring program NACHA requires. No additional compliance tooling needed.

**AMLA harmonization → Jurisdiction-aware rule engine**
Each entity in the system carries a jurisdiction profile. Agent system prompts are dynamically constructed with the applicable ruleset for that entity's domicile and counterparty jurisdictions.

---

## 7. Team Structure

### Core Team (Pilot Phase — 0–6 months)

| Role | Count | Responsibilities |
|---|---|---|
| **Product Lead** | 1 | Product strategy, customer discovery, pilot success definition |
| **ML Engineer — Agent Systems** | 2 | Orchestrator design, agent prompt engineering, tool manifest implementation |
| **ML Engineer — RAG/Retrieval** | 2 | Data ingestion pipelines, vector DB, hybrid retrieval, re-ranking |
| **Backend Engineer** | 2 | API gateway, Firestore state machine, audit log infrastructure |
| **Compliance Domain Expert** | 1 | Regulatory guidance, golden dataset curation, SAR format library |
| **Financial Crime Analyst** | 1 | Eval dataset construction, L2 human-in-loop reviews, pilot institution liaison |
| **Security Engineer** | 1 | CMEK setup, VPC isolation, pen testing scope, NYDFS compliance |
| **QA/Evals Engineer** | 1 | Eval pipeline, L0/L1 automation, LLM-as-judge harness |

Total pilot team: **11 people**

### Expansion Team (Production Phase — 6–18 months)

| Role | Additional Headcount |
|---|---|
| ML Engineers (agent expansion) | +3 |
| Data Engineers (additional source integrations) | +2 |
| Solutions Engineers (institution onboarding) | +2 |
| Compliance Engineers (EU/APAC regulatory support) | +2 |
| Customer Success Managers | +2 |

Total production team: **~22 people**

---

## 8. Phased Roadmap

### Phase 0 — Foundation (Months 1–2)
**Goal:** Infrastructure standing, pilot institution signed, core data pipelines running

- [ ] GCP project setup: VPCs, IAM roles, BigQuery audit tables, Firestore collections
- [ ] Vertex AI Vector Search: multi-tenant namespace structure, first institution namespace
- [ ] Sanctions + adverse news ETL pipelines (OFAC every 4h; Dow Jones every 15min)
- [ ] Audit log schema finalized and reviewed by compliance counsel
- [ ] Capability manifest framework: Apigee gateway + policy enforcement
- [ ] Pilot institution MOU signed; data access agreements in place
- [ ] L0 eval harness operational in CI/CD

**Exit criteria:** Infrastructure deployed, audit log writing to BigQuery, at least one agent returning citations from live sanctions index

---

### Phase 1 — Pilot Agents (Months 3–4)
**Goal:** KYC Onboarding Agent + Transaction Monitoring Agent live at pilot institution

- [ ] KYC Onboarding Agent: document verification flow, PEP/sanctions screening, risk tier output
- [ ] Transaction Monitoring Agent: behavioral baseline ingestion, real-time alert generation
- [ ] Orchestrator v1: routing logic for KYC and monitoring case types
- [ ] QA Validator: hallucination detection, bias checks on KYC tier assignments
- [ ] Human review queue: case delivery UI with evidence bundle + recommended action
- [ ] L1 eval suite: golden dataset (confirmed cases from pilot institution's history)
- [ ] Pilot institution UAT: 2-week parallel run alongside existing compliance workflow

**Exit criteria:** 90-day pilot KPIs baselined; first real cases processed through Antigravity

---

### Phase 2 — Pilot Validation (Month 5–6)
**Goal:** Prove the metrics; gather signal for expansion

- [ ] Measure: STP rate on KYC cases, false-positive rate on AML alerts
- [ ] L2 evals: analyst feedback loop operational (100 cases/week reviewed)
- [ ] Perpetual KYC Agent: event-driven re-review replacing periodic schedule
- [ ] Beneficial Owner Agent: UBO mapping from corporate registries
- [ ] Bias testing: disparate impact analysis on KYC tier assignments
- [ ] Pilot institution CCO sign-off on audit log as regulatory evidence
- [ ] Pilot results package: deck + data for enterprise sales

**Exit criteria:** > 85% STP on KYC, > 35% reduction in AML L1 review time, zero hard-stop violations

---

### Phase 3 — Production Expansion (Months 7–12)
**Goal:** Full agent squad, multi-institution, production-hardened

- [ ] Fraud Detection Agent: multi-vector scoring at payment speed (< 100ms p99)
- [ ] Sanctions Screening Agent: fuzzy matching, alias resolution, NACHA compliance
- [ ] Case Investigator Agent: autonomous L1/L2 with investigation narrative
- [ ] SAR/STR Filing Agent: jurisdiction-specific templates, two-person integrity workflow
- [ ] Regulatory Reporting Agent: CTR, MIS dashboards, reg-change event handling
- [ ] Second institution onboarding: template for multi-tenant deployment
- [ ] SOC 2 Type II audit initiated
- [ ] EU deployment track: AMLA-compliant rule engine, EU AI Act conformity assessment

**Exit criteria:** Full agent squad operational, 2+ institutions in production, SOC 2 in progress

---

### Phase 4 — Scale (Months 13–18)
**Goal:** Self-serve onboarding, EU/APAC expansion, published benchmark

- [ ] Institution self-serve onboarding: config wizard, test environment, go-live checklist
- [ ] EU AMLA compliance: jurisdiction-aware rule injection per entity domicile
- [ ] AUSTRAC Tranche 2: extend KYC agent to gatekeeper professions
- [ ] Published Antigravity Compliance Benchmark: annual FP rate + accuracy report
- [ ] 10+ institutions in production
- [ ] SOC 2 Type II certified

---

## 9. 90-Day Pilot Playbook

### Pilot Scope
Two agents, one institution, 90 days. Strict parallel-run with existing compliance workflow — Antigravity makes recommendations, humans make decisions, both are logged.

**Agents in scope:** KYC Onboarding Agent, Transaction Monitoring Agent
**Institution type:** US bank, $10B–$50B assets, 50–150 compliance FTEs
**Human approval required for:** All KYC tier assignments, all AML alert escalations/closures

### Week-by-Week Plan

**Weeks 1–2: Discovery & Data**
- Kick-off with CCO, CISO, CTO
- Data access: transaction history (12 months), prior KYC case outcomes, confirmed SAR cases
- Map institution's current KYC workflow and AML alert queue process
- Identify top 3 pain points to baseline metrics against
- Complete data processing agreement and CMEK key setup

**Weeks 3–4: Integration**
- Connect core banking API to Transaction Monitor ingestion pipeline
- Load institution namespace in Vertex AI Vector Search
- Ingest institution's internal adverse news and PEP screening configurations
- Deploy human review queue UI in institution's environment
- Run first synthetic test cases through agents

**Weeks 5–6: Parallel Run Start**
- Process new KYC onboarding cases through Antigravity alongside existing workflow
- Transaction Monitor begins generating alerts in parallel (not replacing existing AML system yet)
- Analyst team reviews Antigravity recommendations daily: agree/disagree with reasoning
- Daily feedback sessions: 30 min with compliance team lead

**Weeks 7–10: Measurement & Iteration**
- Baseline established: STP rate, FP rate, time-per-case
- L2 evals running: 100 cases/week reviewed by financial crime analyst
- Adjust risk thresholds based on analyst feedback
- Bias test run: ensure no disparate impact in KYC tier assignments
- Audit log reviewed by institution's compliance counsel

**Weeks 11–12: Validation & Reporting**
- Full 90-day metrics compiled
- CCO presentation: results vs baseline, regulatory evidence package
- Audit log walk-through: demonstrate reconstructibility of every decision
- Go/no-go for Phase 2 expansion
- Pilot results published internally (with institution consent) as sales collateral

### Pilot Success Criteria

| Metric | Target | Measurement Method |
|---|---|---|
| KYC straight-through processing | > 85% of standard cases | Cases closed by agent without analyst intervention / total cases |
| AML L1 review time reduction | > 35% | Average minutes per alert: Antigravity-assisted vs baseline |
| Hard-stop violations | 0 | Audit log query: any action taken without required approval token |
| Analyst agreement rate | > 80% | Cases where analyst confirmed agent recommendation vs overrode |
| Audit log completeness | 100% | Every case has complete trace_id chain from trigger to decision |
| Regulatory evidence package | CCO sign-off | Written confirmation that audit log satisfies FinCEN NPRM documentation requirement |

### Pilot Risk Mitigations

**Risk: Institution data access takes longer than expected**
Mitigation: Start integration planning in Week 1; use synthetic data for agent development while access is being provisioned

**Risk: Analyst team resistant to AI recommendations**
Mitigation: Frame as "AI assistant to reduce noise" not "AI replacing analysts"; track agreement rate to show where agents add value

**Risk: KYC accuracy below target on institution-specific case types**
Mitigation: L2 human review identifies failure modes in Weeks 5–6; fast iteration cycle (new eval + prompt update in < 48 hours)

**Risk: Regulatory examination during pilot**
Mitigation: Audit log is complete from Week 1; parallel run means institution has full human-reviewed record throughout; Antigravity is additive evidence, not a replacement

---

## 10. Risks & Mitigations

| Risk | Probability | Impact | Mitigation |
|---|---|---|---|
| LLM hallucination in SAR narrative | Medium | High | Citation anchoring in output schema; QA Validator checks faithfulness; human sign-off before filing |
| Bias in KYC tier assignments | Medium | High | Disparate impact testing as L1 eval; blocks deploy if ratio < 0.80 |
| Prompt injection via malicious documents | Low | High | Input sanitization at API gateway; agent cannot execute instructions embedded in customer documents |
| Sanctions list staleness during outage | Low | Critical | Fallback to human queue when index freshness > 6 hours; never silently degrade to stale data |
| Institution data leak across namespaces | Very Low | Critical | Infrastructure-level isolation; not configurable; penetration tested quarterly |
| Regulatory change invalidates agent logic | Medium | Medium | Rulemaking feed subscription; auto-regression eval on rule change events |
| Over-reliance — analysts stop thinking critically | Medium | Medium | Analyst agreement rate tracked; alert if > 98% (rubber-stamping signal) |
| Model provider deprecates model version | Low | Medium | Ensemble design; hash-pinned versions; canary testing before production rollout |

---

## 11. Success Metrics & KPIs

### Product KPIs (per institution, quarterly)

| KPI | Definition | Target |
|---|---|---|
| STP Rate | % of KYC cases closed by agent without analyst intervention | > 90% |
| AML False Positive Rate | % of AML alerts that are not genuine suspicious activity | < 20% |
| Case Resolution Time | Average minutes from trigger to closed case (L1) | < 8 min |
| Analyst Agreement Rate | % of cases where analyst confirms agent recommendation | > 80% |
| Zero Hard-Stop Violations | Count of consequential actions taken without approval token | 0 |
| Audit Log Completeness | % of cases with complete trace_id chain | 100% |
| Regulatory Exam Pass Rate | % of sampled decisions satisfying examiner documentation request | > 99% |

### Business KPIs

| KPI | Year 1 Target |
|---|---|
| Pilot institutions signed | 3 |
| Production institutions live | 2 |
| Annual Recurring Revenue | $3M+ |
| Net Revenue Retention | > 120% (product improves with use) |
| Time to onboard new institution | < 6 weeks |
| Customer ROI achieved | 2× within 12 months |

### Engineering KPIs

| KPI | Target |
|---|---|
| Sanctions screening latency p99 | < 100ms |
| KYC onboarding latency p95 | < 3 minutes |
| SAR draft generation | < 8 minutes |
| Eval regression gate | Blocks deploy if L1 score drops > 0.5% |
| Audit log write latency | < 500ms (synchronous with agent response) |
| System uptime | 99.9% |
| CMEK key rotation | Every 90 days, zero downtime |

---

*Google Antigravity — Implementation Plan v1.0 | Confidential | Q2 2026*

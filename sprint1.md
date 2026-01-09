# 🚀 MetroNet AI Manager — 1-Day Sprint Plan (ZeroDB-Native)

**Sprint Goal (Day 0 → Day 1):**

> Deliver a **working AI-powered incident → RCA → remediation loop** backed by ZeroDB tables + vectors, with an LLM NOC assistant demo.

---

## 🕘 Sprint Duration

* **Total:** 8–10 hours
* **Team Assumption:**

  * 1 Platform Engineer
  * 1 AI / Backend Engineer
  * Optional: 1 Frontend (dashboard shell only)

---

## 🧱 Deliverables by End of Day

✅ ZeroDB relational schema live
✅ Vector namespaces provisioned
✅ Incident → RCA → remediation flow working
✅ LLM NOC Assistant answering real questions
✅ Explainability via metadata → tables
✅ Demo script + test data

---

# ⏱️ Hour-by-Hour Sprint Breakdown

---

## **Hour 0–1 — Sprint Kickoff & Environment Setup**

### Objectives

* Lock scope
* Provision ZeroDB project
* Establish namespaces + API access

### Tasks

* Create ZeroDB project
* Configure API keys
* Define environment variables
* Create namespaces:

  * `noc_incidents`
  * `telemetry_patterns`
  * `rca_knowledge`
  * `runbooks`
  * `intent_context`

### Output

✔ ZeroDB project live
✔ Vector namespaces initialized

---

## **Hour 1–2 — Relational Schema Creation (Core Tables)**

### Objectives

Create the **minimum viable truth layer**.

### Tables Created

* `tenants`
* `users`
* `devices`
* `interfaces`
* `links`
* `alarms`
* `incidents`
* `incident_alarms`
* `root_causes`
* `remediation_actions`
* `remediation_executions`

### Tasks

* POST schema via ZeroDB Tables API
* Validate inserts & reads
* Seed test tenant + user

### Output

✔ ZeroDB relational backbone ready
✔ Compliance-safe authoritative data

---

## **Hour 2–3 — Telemetry Rollups + Alarm Generation**

### Objectives

Simulate real network signals.

### Tasks

* Insert synthetic telemetry into `telemetry_rollups`
* Generate alarms based on thresholds
* Link alarms to devices & links
* Create at least 1 cascading alarm scenario

### Output

✔ Telemetry summaries stored
✔ Alarms flowing into ZeroDB

---

## **Hour 3–4 — Incident Creation + Vector Embedding**

### Objectives

Enable incident clustering + similarity.

### Tasks

* Open incident from correlated alarms
* Embed incident description into:

  * `noc_incidents`
* Attach metadata:

  * incident_id
  * severity
  * affected devices

### Output

✔ Incident searchable via vector similarity
✔ Alarm noise visibly reduced

---

## **Hour 4–5 — RCA Knowledge Base + Inference**

### Objectives

Demonstrate AI reasoning + learning.

### Tasks

* Seed `rca_knowledge` with 5–10 known RCA patterns
* Retrieve relevant RCA vectors
* Select top RCA with confidence
* Store result in `root_causes`

### Output

✔ Probabilistic RCA with confidence
✔ Explainability preserved

---

## **Hour 5–6 — Runbooks + Remediation Execution**

### Objectives

Close the loop.

### Tasks

* Seed `runbooks` namespace
* Match RCA → remediation action
* Insert remediation into:

  * `remediation_actions`
* Execute remediation (mock or real)
* Record outcome in:

  * `remediation_executions`

### Output

✔ Closed-loop AI remediation
✔ Human-in-the-loop supported

---

## **Hour 6–7 — LLM-Powered NOC Assistant (RAG)**

### Objectives

Natural language ops interface.

### Tasks

* Build RAG query pipeline across:

  * `noc_incidents`
  * `rca_knowledge`
  * `runbooks`
  * `intent_context`
* Prompt design:

  * “Why did this incident happen?”
  * “How was it fixed?”
* Return:

  * Explanation
  * Confidence
  * Linked IDs

### Output

✔ LLM answers grounded in ZeroDB
✔ No hallucinations

---

## **Hour 7–8 — Explainability, Metrics & Demo Prep**

### Objectives

Make it enterprise-credible.

### Tasks

* Add metadata → table lookups
* Validate traceability:

  * AI output → vector → table row
* Capture demo metrics:

  * Alarm reduction
  * MTTR improvement

### Output

✔ End-to-end explainability
✔ Demo script ready

---

## **Optional Hour 8–10 — UI Shell or API Docs**

**If time permits:**

* Simple dashboard:

  * Incidents
  * RCA
  * Actions
* OR
* API documentation + diagrams

---

# 📦 What This Day *Does Not* Try to Do

❌ Full SDN control
❌ Production-grade ML training
❌ Vendor integrations
❌ UI polish

Those are **post-Day-1 sprints**, now justified.

---

# 🎯 Day-1 Success Criteria

| Metric                | Target         |
| --------------------- | -------------- |
| Incident → RCA        | < 30 sec       |
| Alarm Noise Reduction | ≥ 60%          |
| AI Explainability     | 100% traceable |
| Remediation Execution | Working        |
| Demo Readiness        | Yes            |

---

## 🔥 Why This Works

* ZeroDB handles **truth + memory**
* Vectors handle **reasoning**
* Metadata binds them cleanly
* AI is **useful on Day 1**, not aspirational

---



# 📦 MetroNet AI Manager — Product Backlog

## Backlog Structure

* **Epics** = Architectural capability slices
* **User Stories** = Observable user or system value
* Stories are written to be:

  * ZeroDB-native
  * Vector-aware
  * Explainability-first
  * Closed-loop ready

---

## 🧱 EPIC 1 — Multi-Tenant Identity & Access (Foundation)

**Goal:** Secure, compliant multi-tenant isolation across all layers.

### User Stories

1. **As a platform admin**, I can create and manage tenants so each customer has isolated data.
2. **As a user**, I can authenticate and be assigned a role within a tenant.
3. **As a NOC manager**, I can control which users can approve remediation actions.
4. **As a compliance officer**, I can audit all user actions via event logs.
5. **As the system**, I store all identity changes as immutable events.

**ZeroDB Tables**

* `tenants`
* `users`
* `events (identity_audit)`

---

## 🌐 EPIC 2 — Network Inventory & Logical Topology

**Goal:** Unified, vendor-agnostic representation of metro networks.

### User Stories

1. **As a network engineer**, I can register network domains (metro, edge, core).
2. **As the system**, I store devices with vendor, model, OS, and management IP.
3. **As the system**, I map interfaces and logical links without enforcing physical graph constraints.
4. **As an AI model**, I can reference topology metadata for RCA reasoning.
5. **As an operator**, I can see device and link health status.

**ZeroDB Tables**

* `network_domains`
* `devices`
* `interfaces`
* `links`

---

## 🛣️ EPIC 3 — Services, Paths & Traffic Engineering

**Goal:** Represent services and their computed paths for optimization and enforcement.

### User Stories

1. **As a network engineer**, I can define logical services with SLA profiles.
2. **As the system**, I compute and store service paths.
3. **As the system**, I store ordered path hops for explainability.
4. **As an AI agent**, I can compare active vs alternate paths during incidents.
5. **As a NOC engineer**, I can see which services are impacted by a device or link.

**ZeroDB Tables**

* `services`
* `service_paths`
* `path_hops`

---

## 🎯 EPIC 4 — Intent-Based Networking

**Goal:** Shift from manual configs to declarative outcomes.

### User Stories

1. **As a network architect**, I can define high-level intents (latency, availability).
2. **As the system**, I continuously evaluate intents against telemetry summaries.
3. **As the system**, I log intent violations with severity and details.
4. **As an AI agent**, I use intent context during RCA and remediation selection.
5. **As a NOC manager**, I can view intent violation trends.

**ZeroDB Tables**

* `intents`
* `intent_violations`

**Vector Namespace**

* `intent_context`

---

## 📊 EPIC 5 — Telemetry Summarization & Observability

**Goal:** Convert raw telemetry into AI-usable summaries.

### User Stories

1. **As the system**, I ingest telemetry rollups instead of raw streams.
2. **As the system**, I store per-entity metrics by time window.
3. **As an AI model**, I retrieve rollups for anomaly detection.
4. **As an operator**, I can see recent health trends per device and link.

**ZeroDB Tables**

* `telemetry_rollups`

**Vector Namespace**

* `telemetry_patterns`

---

## 🚨 EPIC 6 — Alarm Management & Noise Reduction

**Goal:** Reduce alarm floods with explainable clustering.

### User Stories

1. **As the system**, I create alarms from telemetry and external sources.
2. **As the system**, I embed alarm descriptions into the incident similarity space.
3. **As an AI model**, I cluster alarms into probable incidents.
4. **As a NOC engineer**, I see suppressed secondary alarms.
5. **As a manager**, I can measure alarm noise reduction over time.

**ZeroDB Tables**

* `alarms`

**Vector Namespace**

* `noc_incidents`

---

## 🔥 EPIC 7 — Incident Lifecycle Management

**Goal:** Structured, explainable incident handling.

### User Stories

1. **As a NOC engineer**, I can open and track incidents.
2. **As the system**, I associate alarms with incidents.
3. **As the system**, I embed incidents for similarity search.
4. **As an AI agent**, I retrieve similar past incidents for guidance.
5. **As a manager**, I track MTTR and incident outcomes.

**ZeroDB Tables**

* `incidents`
* `incident_alarms`

**Vector Namespace**

* `noc_incidents`

---

## 🧠 EPIC 8 — AI Root Cause Analysis (RCA)

**Goal:** Probabilistic, explainable RCA using history + topology.

### User Stories

1. **As an AI agent**, I infer probable root causes with confidence scores.
2. **As the system**, I store RCA results linked to incidents.
3. **As an AI agent**, I reference historical RCA knowledge.
4. **As a NOC engineer**, I see why the AI chose a root cause.
5. **As a compliance officer**, I can audit RCA decisions.

**ZeroDB Tables**

* `root_causes`

**Vector Namespace**

* `rca_knowledge`

---

## 🤖 EPIC 9 — Automated Remediation & Closed Loop Control

**Goal:** Move from suggestion → execution → learning.

### User Stories

1. **As a system**, I store remediation actions and constraints.
2. **As an AI agent**, I select remediation actions based on context.
3. **As a NOC engineer**, I approve or reject AI-recommended actions.
4. **As the system**, I execute actions and record results.
5. **As an AI model**, I learn from remediation outcomes.

**ZeroDB Tables**

* `remediation_actions`
* `remediation_executions`

**Vector Namespace**

* `runbooks`

---

## 💬 EPIC 10 — LLM-Powered NOC Assistant

**Goal:** Natural language interface grounded in ZeroDB.

### User Stories

1. **As a NOC engineer**, I can ask why an incident occurred.
2. **As a NOC engineer**, I can request a fix in natural language.
3. **As the assistant**, I retrieve incidents, RCA, runbooks, and intents.
4. **As the assistant**, I cite confidence and supporting data.
5. **As a manager**, I can request daily or weekly summaries.

**RAG Namespaces**

* `noc_incidents`
* `rca_knowledge`
* `runbooks`
* `intent_context`

---

## 🔌 EPIC 11 — Workflow & Ticketing Integrations

**Goal:** Seamless integration with enterprise tooling.

### User Stories

1. **As the system**, I create ServiceNow/Jira tickets automatically.
2. **As the system**, I attach RCA and remediation suggestions.
3. **As the system**, I auto-close tickets when incidents resolve.
4. **As a NOC manager**, I track ticket automation metrics.

---

## 🛡️ EPIC 12 — Governance, Security & Compliance

**Goal:** Enterprise-grade trust and auditability.

### User Stories

1. **As a security officer**, I enforce RBAC across all APIs.
2. **As a compliance officer**, I view immutable audit logs.
3. **As the system**, I isolate tenants at data and vector levels.
4. **As an auditor**, I trace every AI decision to raw data.
5. **As an operator**, I deploy with zero-downtime upgrades.

---

## 📈 EPIC 13 — Reporting & Executive Visibility

**Goal:** Translate ops data into business outcomes.

### User Stories

1. **As a CTO**, I view SLA compliance trends.
2. **As a NOC manager**, I see automation effectiveness metrics.
3. **As leadership**, I track MTTR, MTTD, and alarm reduction.
4. **As a product owner**, I evaluate AI accuracy over time.

---



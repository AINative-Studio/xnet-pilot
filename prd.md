# Product Requirements Document (PRD)

**Product Name:** (Working) *MetroNet AI Manager*
**Version:** 1.0
**Author:** —
**Target Release:** TBD
**Product Type:** Network Management / SDN / AI Ops (AIOps)

---

## 1. Purpose & Vision

### 1.1 Problem Statement

Metro Area Networks (MANs) operated by telecoms, ISPs, utilities, and smart-city operators are:

* Highly heterogeneous (fiber, IP/MPLS, Ethernet, microwave, 5G backhaul)
* Increasingly dynamic due to virtualization, slicing, and edge computing
* Operationally expensive due to manual, reactive NOC processes

Network Operations Centers (NOCs) are overwhelmed by:

* Alarm floods
* Manual triage and root-cause analysis
* Repetitive configuration and remediation tasks
* SLA-driven incident pressure

### 1.2 Vision

Build an **AI-powered SDN management platform** that:

* **Centralizes control and visibility** of metro networks
* **Automates detection, diagnosis, and remediation** of network issues
* **Offloads 50–70% of routine NOC tasks** within the first year
* Enables **intent-based, closed-loop network operations**

---

## 2. Goals & Success Metrics

### 2.1 Primary Goals

1. Reduce manual NOC workload via AI-driven automation
2. Improve network availability and SLA compliance
3. Shorten Mean Time to Detect (MTTD) and Mean Time to Resolve (MTTR)
4. Provide a scalable SDN control layer for MANs

### 2.2 Key Success Metrics (KPIs)

| Metric                        | Baseline  | Target   |
| ----------------------------- | --------- | -------- |
| MTTD                          | 10–15 min | < 1 min  |
| MTTR                          | 1–4 hours | < 15 min |
| Alarm Noise Reduction         | 0%        | ≥ 70%    |
| Automated Incident Resolution | 0%        | ≥ 40%    |
| NOC Tickets per Node          | Baseline  | −50%     |
| SLA Violations                | Baseline  | −30%     |

---

## 3. Target Users & Personas

### 3.1 Primary Personas

1. **NOC Engineer (L1/L2)**

   * Monitors alarms, handles incidents
   * Wants fewer false alarms and guided actions

2. **Network Engineer (L3)**

   * Designs, configures, and optimizes the network
   * Wants intent-based configuration and deep insights

3. **NOC Manager**

   * Owns SLAs, staffing, and escalation
   * Wants automation, reporting, and predictability

4. **CTO / Network Architect**

   * Responsible for scalability and cost
   * Wants future-proof SDN and AI strategy

---

## 4. Scope

### 4.1 In Scope

* Metro IP/MPLS, Ethernet, optical aggregation
* SDN controller integration
* AI-driven monitoring, analytics, and automation
* NOC workflow automation
* Multi-vendor support

### 4.2 Out of Scope (Phase 1)

* Customer-facing portals
* Access network (CPE) management
* Billing and charging
* Core backbone optimization (beyond MAN)

---

## 5. High-Level Architecture

### 5.1 Architecture Overview

```
+--------------------------------------------------+
|                    UI / API Layer                |
|  Dashboards | Intent Engine | Reports | APIs     |
+--------------------------------------------------+
|              AI & Automation Layer               |
|  Anomaly Detection | RCA | Prediction | LLM Ops  |
+--------------------------------------------------+
|             SDN Control & Orchestration          |
|  Path Computation | Policy Engine | Flow Control |
+--------------------------------------------------+
|              Data & Telemetry Layer              |
|  Streaming Telemetry | Logs | SNMP | NetFlow     |
+--------------------------------------------------+
|               Network Infrastructure             |
|  Routers | Switches | Optical | Wireless         |
+--------------------------------------------------+
```

---

## 6. Functional Requirements

### 6.1 SDN Management & Control

#### 6.1.1 Network Abstraction

* Unified logical topology view across vendors and technologies
* Abstract physical devices into logical nodes and links
* Support:

  * OpenFlow
  * NETCONF/YANG
  * gNMI
  * REST APIs
  * Legacy SNMP (read-only minimum)

#### 6.1.2 Path & Policy Control

* Centralized path computation
* Traffic engineering for:

  * Latency
  * Bandwidth
  * Packet loss
* Policy-based forwarding rules
* Support for fast reroute and protection paths

---

### 6.2 Telemetry, Monitoring & Observability

#### 6.2.1 Data Collection

* Real-time streaming telemetry (sub-second)
* Metrics:

  * Interface stats
  * Queue depth
  * Latency, jitter, packet loss
  * CPU, memory, temperature
* Logs and events ingestion

#### 6.2.2 Topology & Health Visualization

* Live metro topology map
* Per-link and per-node health indicators
* SLA heatmaps

---

### 6.3 AI-Driven NOC Offloading (Core Differentiator)

#### 6.3.1 Alarm Correlation & Noise Reduction

* ML-based alarm clustering
* Suppress secondary and cascading alarms
* Correlate across layers (L1–L4)

**Requirements:**

* ≥70% alarm reduction
* Explainable correlation results

---

#### 6.3.2 Anomaly Detection

* Unsupervised models to detect:

  * Traffic pattern deviations
  * Latency spikes
  * Microbursts
* Baseline learned per:

  * Time of day
  * Day of week
  * Service class

---

#### 6.3.3 Root Cause Analysis (RCA)

* Probabilistic and graph-based RCA
* Identify most likely fault domain:

  * Device
  * Link
  * Configuration
  * External dependency
* Output confidence scores

---

#### 6.3.4 Predictive Analytics

* Predict:

  * Link congestion
  * Device failures
  * SLA violations
* Time horizons:

  * Minutes
  * Hours
  * Days

---

#### 6.3.5 Automated Remediation (Closed Loop)

* AI recommends remediation actions
* Actions can be:

  * Auto-executed
  * Human-approved
* Examples:

  * Reroute traffic
  * Adjust QoS policies
  * Restart services
  * Roll back configurations

---

#### 6.3.6 LLM-Powered NOC Assistant

* Natural language interface for NOC staff
* Capabilities:

  * “Why is latency high in Zone A?”
  * “Fix congestion on link X”
  * “Summarize today’s incidents”
* Uses:

  * Network state
  * Runbooks
  * Historical incidents

---

### 6.4 Intent-Based Networking

* Operators define **what** they want, not **how**
* Example intents:

  * “Keep latency < 5ms for 5G backhaul”
  * “Ensure 99.99% availability for enterprise customers”
* System continuously enforces intent using closed-loop control

---

### 6.5 Workflow & Ticketing Integration

* Integrations:

  * ServiceNow
  * Jira
  * PagerDuty
* Auto-ticket creation with RCA and suggested fix
* Auto-close tickets when resolved

---

## 7. Non-Functional Requirements

### 7.1 Performance

* Telemetry ingestion: ≥1M metrics/sec
* Control-plane latency: <100ms
* UI updates: near real-time (<2s)

### 7.2 Scalability

* Support:

  * 10,000+ network devices
  * 100,000+ links
* Horizontal scaling via microservices

### 7.3 Availability & Resilience

* Platform availability ≥99.99%
* Active-active deployment
* No single point of failure

### 7.4 Security

* Role-based access control (RBAC)
* Multi-tenant isolation
* TLS everywhere
* Audit logs for all changes
* Zero-trust API access

### 7.5 Compliance

* Support for:

  * ISO 27001
  * SOC 2
  * GDPR (where applicable)

---

## 8. AI & Data Requirements

### 8.1 Data Sources

* Telemetry streams
* Historical alarms/incidents
* Configuration changes
* Maintenance windows

### 8.2 Model Types

* Time-series anomaly detection
* Graph neural networks (topology-aware)
* Bayesian RCA models
* LLMs for NOC interaction

### 8.3 Explainability

* Every AI decision must provide:

  * Reasoning summary
  * Confidence score
  * Supporting data points

---

## 9. User Experience (UX) Requirements

* Role-based dashboards
* Minimal alarm fatigue
* “One-click” remediation
* Drill-down from metro view → link → packet stats
* Dark-mode NOC-friendly UI

---

## 10. Deployment & Operations

### 10.1 Deployment Models

* On-prem (telco-grade)
* Private cloud
* Hybrid

### 10.2 Upgrade Strategy

* Zero-downtime upgrades
* Canary releases for AI models

---

## 11. Risks & Mitigations

| Risk                    | Mitigation                        |
| ----------------------- | --------------------------------- |
| AI mistrust by NOC      | Explainability, human-in-the-loop |
| Multi-vendor complexity | Strong abstraction layer          |
| False positives         | Progressive model training        |
| Automation errors       | Guardrails + rollback             |

---

## 12. Roadmap (High-Level)

### Phase 1 – Foundation (0–6 months)

* Telemetry ingestion
* SDN control
* Monitoring & visualization
* Basic anomaly detection

### Phase 2 – AI Ops (6–12 months)

* Alarm correlation
* RCA
* Predictive analytics
* Assisted remediation

### Phase 3 – Autonomous NOC (12–24 months)

* Closed-loop automation
* Intent-based networking
* Advanced LLM NOC assistant

---

## 13. Open Questions / Customization Points

* Target customer segment (Tier-1 telco vs municipal vs ISP)?
* Required vendor integrations?
* Regulatory constraints?
* Preference for open-source vs proprietary SDN controllers?

---


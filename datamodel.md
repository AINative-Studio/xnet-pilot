* **PostgreSQL tables** (via `/database/tables`)
* **Vector namespaces** (via `/embeddings/*` and `/database/vectors/*`)
* **384-dimension default embeddings** (BAAI/bge-small-en-v1.5)
* **JSON metadata instead of foreign-key joins where appropriate**

---

# 1. ZeroDB-Native Data Architecture

ZeroDB acts as a **unified data plane**, not a classic polyglot stack. The model is therefore simplified into **three first-class layers**:

| Layer      | ZeroDB Feature         | Purpose                         |
| ---------- | ---------------------- | ------------------------------- |
| Relational | PostgreSQL Tables      | Authoritative system of record  |
| Vector     | Embeddings + Vector DB | Similarity, RCA, RAG, AI Ops    |
| Events     | Events API / Tables    | Telemetry summaries, audit, ops |

> **Key Principle:**
>
> * **Tables = truth & compliance**
> * **Vectors = reasoning & similarity**
> * **Metadata links everything**

---

# 2. Relational Data Model (ZeroDB Tables)

All tables are created via:

```
POST /v1/public/{project_id}/database/tables
```

Foreign keys are **logical**, not enforced at DB level (ZeroDB best practice).

---

## 2.1 Tenancy & Identity

### `tenants`

```sql
tenant_id UUID PRIMARY KEY
name TEXT NOT NULL
tier TEXT
created_at TIMESTAMP DEFAULT NOW()
```

---

### `users`

```sql
user_id UUID PRIMARY KEY
tenant_id UUID
email TEXT UNIQUE
role TEXT
status TEXT
created_at TIMESTAMP DEFAULT NOW()
```

---

## 2.2 Network Inventory & Topology

### `network_domains`

```sql
domain_id UUID PRIMARY KEY
tenant_id UUID
name TEXT
type TEXT -- metro, edge, core
```

---

### `devices`

```sql
device_id UUID PRIMARY KEY
domain_id UUID
vendor TEXT
model TEXT
os_version TEXT
mgmt_ip TEXT
device_type TEXT
status TEXT
created_at TIMESTAMP DEFAULT NOW()
```

---

### `interfaces`

```sql
interface_id UUID PRIMARY KEY
device_id UUID
name TEXT
speed_mbps INTEGER
admin_state TEXT
oper_state TEXT
```

---

### `links`

```sql
link_id UUID PRIMARY KEY
src_interface_id UUID
dst_interface_id UUID
link_type TEXT
capacity_mbps INTEGER
status TEXT
```

> 🔹 **Note:**
> Graph traversal (for RCA) is handled via **vector + metadata + optional external graph logic**, not Neo4j.

---

## 2.3 Services, Paths & Intents

### `services`

```sql
service_id UUID PRIMARY KEY
tenant_id UUID
name TEXT
service_type TEXT
sla_profile JSONB
```

---

### `service_paths`

```sql
path_id UUID PRIMARY KEY
service_id UUID
active BOOLEAN
computed_by TEXT
created_at TIMESTAMP DEFAULT NOW()
```

---

### `path_hops`

```sql
path_id UUID
hop_order INTEGER
device_id UUID
interface_id UUID
```

---

### `intents`

```sql
intent_id UUID PRIMARY KEY
tenant_id UUID
intent_type TEXT
target JSONB
constraints JSONB
active BOOLEAN
created_at TIMESTAMP DEFAULT NOW()
```

---

### `intent_violations`

```sql
violation_id UUID PRIMARY KEY
intent_id UUID
detected_at TIMESTAMP
severity TEXT
details JSONB
```

---

## 2.4 Telemetry, Alarms & Incidents

> Raw telemetry stays outside ZeroDB (Prometheus, gNMI).
> ZeroDB stores **summaries, events, and AI-relevant slices**.

---

### `telemetry_rollups`

```sql
rollup_id UUID PRIMARY KEY
entity_type TEXT -- device, link
entity_id UUID
metric TEXT
window TEXT -- 1m, 5m
avg_value FLOAT
max_value FLOAT
timestamp TIMESTAMP
```

---

### `alarms`

```sql
alarm_id UUID PRIMARY KEY
source_type TEXT
source_id UUID
severity TEXT
alarm_type TEXT
triggered_at TIMESTAMP
cleared_at TIMESTAMP
raw_payload JSONB
```

---

### `incidents`

```sql
incident_id UUID PRIMARY KEY
tenant_id UUID
status TEXT
priority TEXT
created_at TIMESTAMP
closed_at TIMESTAMP
```

---

### `incident_alarms`

```sql
incident_id UUID
alarm_id UUID
```

---

### `root_causes`

```sql
rca_id UUID PRIMARY KEY
incident_id UUID
entity_type TEXT
entity_id UUID
confidence FLOAT
model_version TEXT
```

---

## 2.5 Automation & Remediation

### `remediation_actions`

```sql
action_id UUID PRIMARY KEY
action_type TEXT
parameters JSONB
requires_approval BOOLEAN
```

---

### `remediation_executions`

```sql
execution_id UUID PRIMARY KEY
incident_id UUID
action_id UUID
status TEXT
executed_by TEXT -- ai, human
executed_at TIMESTAMP
result JSONB
```

---

# 3. Vector Data Model (ZeroDB Embeddings)

All vectors use:

* **Model:** `BAAI/bge-small-en-v1.5`
* **Dimensions:** **384**
* **Stored via:** `/embeddings/embed-and-store` OR `/database/vectors/upsert`
* **Namespace = logical collection**

---

## 3.1 Vector Design Rules (ZeroDB-Specific)

✔ One **embedding model per namespace**
✔ Vectors are **derived**, never authoritative
✔ Relational IDs live in **metadata**
✔ Rebuildable at any time

---

## 3.2 Vector Namespaces & Schemas

---

### 3.2.1 Alarm & Incident Similarity

**Namespace:** `noc_incidents`

**Purpose:**

* Alarm noise reduction
* Incident clustering
* Similar incident retrieval

**Stored via:** `/embeddings/embed-and-store`

```json
{
  "id": "incident_123",
  "text": "High latency and packet loss detected on metro ring A affecting 5G backhaul",
  "metadata": {
    "incident_id": "incident_123",
    "severity": "critical",
    "affected_domain": "metro-east",
    "device_ids": ["dev_1", "dev_2"]
  }
}
```

---

### 3.2.2 Telemetry Pattern Embeddings

**Namespace:** `telemetry_patterns`

**Purpose:**

* Anomaly detection
* Pattern similarity
* Predictive analytics

```json
{
  "id": "link_456_2025-01-14T10:00",
  "text": "Link utilization spike: avg=92%, jitter=18ms, loss=1.2%",
  "metadata": {
    "entity_type": "link",
    "entity_id": "link_456",
    "time_window": "5m",
    "baseline": "weekday-morning"
  }
}
```

---

### 3.2.3 Root Cause Knowledge Base (RCA Memory)

**Namespace:** `rca_knowledge`

**Purpose:**

* AI RCA inference
* Explainability
* Learning from history

```json
{
  "id": "rca_optical_flap",
  "text": "Intermittent optical power loss causes cascading IP alarms in metro rings",
  "metadata": {
    "entity_type": "link",
    "confidence": 0.87,
    "symptoms": ["packet_loss", "link_flap"]
  }
}
```

---

### 3.2.4 Runbooks & SOPs (LLM Tooling)

**Namespace:** `runbooks`

**Purpose:**

* LLM NOC assistant
* Automated remediation selection

```json
{
  "id": "runbook_reroute_congestion",
  "text": "If sustained congestion >90% for 5 minutes, reroute traffic via backup path.",
  "metadata": {
    "action_type": "reroute",
    "risk_level": "medium",
    "approval_required": false
  }
}
```

---

### 3.2.5 Intent & SLA Context

**Namespace:** `intent_context`

**Purpose:**

* Intent violation reasoning
* SLA explanation

```json
{
  "id": "intent_latency_5g",
  "text": "5G backhaul latency must remain under 5ms for enterprise slice",
  "metadata": {
    "intent_id": "intent_789",
    "service_type": "5g",
    "sla": "latency"
  }
}
```

---

# 4. ZeroDB-Aligned Data Flow Examples

## Incident → AI → Remediation

1. Telemetry summarized → `telemetry_rollups`
2. Alarm created → `alarms`
3. Incident opened → `incidents`
4. Incident embedded → `noc_incidents` namespace
5. Similar incidents retrieved (vector search)
6. RCA vectors consulted → `rca_knowledge`
7. Runbook matched → `runbooks`
8. Action executed → `remediation_executions`

---

## LLM NOC Assistant (RAG)

Search namespaces:

* `noc_incidents`
* `rca_knowledge`
* `runbooks`
* `intent_context`

All results are **grounded via metadata → relational IDs**.

---

# 5. Governance, Scaling & Cost Alignment

| Data Type      | Strategy                |
| -------------- | ----------------------- |
| Tables         | Long-term retention     |
| Vectors        | Rebuildable, prune old  |
| Namespaces     | One per use case        |
| Embeddings     | Always 384-dim          |
| Explainability | Metadata → table lookup |

---

# 6. Why This Fits ZeroDB Perfectly

✔ Uses **native ZeroDB APIs only**
✔ No dimension mismatches
✔ RAG-ready out of the box
✔ Clear separation of truth vs inference
✔ Cost-efficient (384-dim everywhere)
✔ Easy to evolve models later

---


Vulture: Architectural Description

Author: Ernesto M. Alfonso Campos
Version: Draft v1.1 (technical review)
License: GNU GPL v3
Date: 15/01/26


# Executive Summary

Vulture is a defensive system designed for anomaly detection, probabilistic correlation, and the generation of auditable explanations regarding behavior on Linux hosts. Its structure is hierarchical, divided into lower, middle, and upper levels, with an emphasis on modularity and evidence orientation: every alert must be traceable directly to the raw telemetry artifacts that support it.

This document explores the overall architecture, the responsibilities assigned to each layer, the data flows that connect everything, the essential operational requirements, and the guiding technical and ethical constraints. It is part of the repository at docs/architecture/architecture.md. For details on threat models, ethical considerations, and report schemas, please refer to the docs/threat-model/ folders, docs/ethics/, and the specs/report-schema.md file, respectively.


---
# Table of Contents 

**Table of Contents**:

## Table of Contents

1. [Scope and Audience](#1-scope-and-audience)
2. [Design Principles](#2-design-principles)
3. [Architectural Overview (Logical View)](#3-architectural-overview-logical-view)
4. [Components and Responsibilities](#4-components-and-responsibilities)
   - [Lower Level: Observation and Behavioral Index Generation](#41-lower-level-observation-and-behavioral-index-generation)
   - [Middle Level: Correlation and Probabilistic Inference](#42-middle-level-correlation-and-probabilistic-inference)
   - [Upper Level: Semantic Interpretation and Explainable Reporting](#43-upper-level-semantic-interpretation-and-explainable-reporting)
5. [Telemetry, Ingestion, and Storage](#5-telemetry-ingestion-and-storage)
6. [Internal Communications and Security](#6-internal-communications-and-security)
7. [Report Schema (Minimal Example)](#7-report-schema-minimal-example)
8. [Metrics and Evaluation](#8-metrics-and-evaluation)
9. [Limitations and Assumptions](#9-limitations-and-assumptions)
10. [Critical Risks and Mitigations](#10-critical-risks-and-mitigations)

This document is primarily aimed at developers, detection engineers, security operations center (SOC) operators, and technical reviewers. It does not delve into specific implementations of agents, deployment scripts, or operational playbooks; those elements are reserved for the source code sections in src/ or playbooks/ once created. Its main purpose is to serve as a solid reference for design, implementation, and future reviews.

---
# 2. Design Principles

Vulture's design is grounded in a defensive philosophy that prioritizes human control: the system acts as an assistant to analysts, without making automatic remediation decisions unless explicitly approved by a human. It maintains a clear hierarchy to separate responsibilities based on levels of abstraction, ensuring that each alert includes verifiable references to the underlying telemetry artifacts. It adopts a Zero Trust approach in its operations, relying on multiple correlations rather than isolated signals. Additionally, it promotes modularity and extensibility, allowing components such as telemetry sources, correlation engines, storages, and explanation generators to be interchangeable. Finally, it incorporates continuous operational measurements, strict limits on agent overhead, and adversarial testing to ensure robustness and security.

---
# 3. Architectural Overview (Logical View)

The architecture flows logically from initial data collection to the delivery of actionable insights. It begins with collectors like eBPF, auditd, inotify, and netlink, which feed an ingestion pipeline. From there, it proceeds to the lower level for local detection, then to the middle level for correlation and prioritization, and culminates in the upper level with semantic interpretation and clear explanations. Finally, results are exposed via APIs or JSON reports, intended for analysts, SIEM systems, or SOAR, always in read-only mode.

Internal transport formats use normalized JSON, as detailed in specs/report-schema.md. For low-latency local communications, Unix sockets are employed, while channels between remote services are protected with authentication and encryption.

---
# 4. Components and Responsibilities

## 4.1 Lower Level: Observation and Behavioral Index Generation

At this level, the focus is on collecting granular telemetry, building temporal features, and emitting local events or alerts using rule-based and heuristic detection models, explicitly avoiding unsupervised ML algorithms such as Isolation Forest.

Recommended sources include eBPF for syscalls and kernel probes, auditd and journald for executions and authentications, inotify along with selective hashing for critical file events, procfs, cgroups, and namespaces for monitoring process trees, and netlink or conntrack for network metadata.

Exemplary agents might include one for syscalls that evaluates deterministic rules over sequences and temporal distributions, another for processes that applies invariant-based rules on forks and execs, one for memory that checks threshold- and pattern-based conditions, another for filesystem that enforces policy rules on accesses to sensitive paths, one for IPC that observes communication patterns through predefined signatures, and a historical agent for logging configuration changes and past accesses.

Outputs consist of normalized JSON events including an ID, timestamp, host, feature window, matched rule identifiers and rule confidence scores, and evidence references. Local JSON reports are also generated to explain which rules were triggered, under what conditions, and based on which raw telemetry artifacts an observation was considered suspicious.

Operationally, it is crucial to measure and limit probe overhead in terms of CPU and memory, with configurable sampling and aggregation options based on the host profile.

## 4.2 Middle Level: Correlation and Probabilistic Inference

Here, signals from multiple sources are aggregated to estimate attack hypotheses through temporal and probabilistic correlations, prioritizing alerts accordingly. Key components include an aggregation and windowing engine for handling temporal windows per entity, a graph store for causal relationships between events, processes, and hosts, and a Bayesian engine for combining probabilities with adjustable priors.

The output is an enriched alert incorporating posterior probability, MITRE mappings, related events, recommended priority, and evidence references. It is important to subject weights and priors to ongoing validation, enabling recalibrations through ModelOps practices, and to store confidence metadata along with model versions.

## 4.3 Upper Level: Semantic Interpretation and Explainable Reporting

This level handles the production of auditable explanations, mapping events to frameworks like MITRE ATT&CK, and creating accessible summaries for analysts. A suggested operational pattern is controlled Retrieval-Augmented Generation (RAG), where evidence fragments and local knowledge bases are retrieved, and an LLM generates explanations strictly anchored to that data. To ensure integrity, prompts, evidence references, and template versions are logged; all explanations must explicitly cite artifacts.

The LLM is limited to explanations and does not make decisions; any remediation suggestions are presented as proposed commands for human validation. In on-premise deployments, quantized models of 8 to 16 bits are recommended, evaluating any precision degradation. Evaluations are kept offline, with test suites and adversarial inputs.

---
# 5. Telemetry, Ingestion, and Storage

Ingestion is designed decoupled, using brokers like Kafka or lightweight alternatives, or local queues to handle fault tolerance and load peaks. All data is normalized according to a common schema, validated with JSON Schema in continuous integration pipelines.

For storage, options like ClickHouse or Timescale handle time series, Elastic or ClickHouse facilitate searches, and Neo4j or JanusGraph support graphs. Security includes encryption at rest, role-based access control (RBAC), and signatures for critical artifacts.

---
# 6. Internal Communications and Security

Local communications leverage Unix sockets to minimize latency between collectors and pipelines. For remote connections, mutual TLS is implemented with strict authentication and authorization. Integrity controls such as checksums and signatures protect artifacts, along with continuous monitoring of collector integrity. In terms of privacy, anonymization or selective hashing of personally identifiable information (PII) is applied, with retention policies defined in research/limitations.md.

---
# 7. Report Schema (Minimal Example)

{
  "id": "evt-20260110-0001",
  "timestamp": "2026-01-10T12:34:56Z",
  "host": "host01",
  "type": "rule_based_event",
  "features": {
    "syscall_sequence": ["execve", "open", "write", "connect"],
    "cpu_pct": 85
  },
  "matched_rules": ["FS_WRITE_SENSITIVE_PATH", "PROC_UNUSUAL_EXEC"],
  "posterior_probability": 0.91,
  "mitre": ["T1547", "T1059"],
  "evidence_refs": ["evt-0000", "log-45321"],
  "recommended_action": "isolate_host (manual)"
}

---
# 8. Metrics and Evaluation

For detection evaluation, precision, recall, and F1 are measured per rule family and threat category. Operationally, false positives per host per day, mean time to detection (MTTD), and end-to-end ingestion latency are tracked. In performance, probe overhead is monitored as a percentage of CPU and memory, along with ingestion throughput. For explainability, the acceptance rate of explanations by analysts is calculated.

---
# 9. Limitations and Assumptions (Refer to research/assumptions.md)

It is assumed that collectors have appropriate permissions and that there is no total adversarial control over telemetry. LLMs are used only for explanations, with outputs anchored and audited. The system does not replace enterprise response processes or compliance controls.

---
# 10. Critical Risks and Mitigations (Summary)

Among risks, telemetry manipulation is mitigated with external collectors, signatures, and verified integrity. LLM hallucinations are controlled through anchored RAG, prompt logging, and human validation. Graph bottlenecks are addressed with partitioning, TTLs, and rollups. False positives are managed with rule tuning and analyst feedback loops. For high availability, single points of failure in brokers and graph databases are avoided through clusters and replicas.


 



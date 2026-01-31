Vulture — Assumptions

Author: Ernesto M. Alfonso Campos
Version: 1.0
License: GNU GPL v3
Date: 19/01/26


---

# Table Of Contents

## Table of Contents:

- [Introduction](#introduction)
- [1. Technical assumptions](#1-technical-assumptions)
  - [1.1 Kernel-level instrumentation privileges](#11-kernel-level-instrumentation-privileges)
  - [1.2 Relative integrity of telemetry](#12-relative-integrity-of-telemetry)
  - [1.3 Constrained use of large language models (LLMs)](#13-constrained-use-of-large-language-models-llms)
  - [1.4 Local communication channels](#14-local-communication-channels)
  - [1.5 Linux kernel support and observability](#15-linux-kernel-support-and-observability)
- [2. Operational assumptions](#2-operational-assumptions)
  - [2.1 Defined target environment](#21-defined-target-environment)
  - [2.2 Human-in-the-loop operation](#22-human-in-the-loop-operation)
  - [2.3 Reasonable resource availability](#23-reasonable-resource-availability)
  - [2.4 Integration with existing processes](#24-integration-with-existing-processes)
  - [2.5 Operator statistical literacy](#25-operator-statistical-literacy)
- [3. Ethical assumptions](#3-ethical-assumptions)
  - [3.1 Defensive use only](#31-defensive-use-only)
  - [3.2 Not a surveillance system](#32-not-a-surveillance-system)
  - [3.3 Data minimization and privacy protection](#33-data-minimization-and-privacy-protection)
  - [3.4 Explainability & auditability requirement](#34-explainability--auditability-requirement)
- [4. Implementation assumptions](#4-implementation-assumptions)
  - [4.1 Modular, hierarchical architecture](#41-modular-hierarchical-architecture)
  - [4.2 Structured data and schema validation](#42-structured-data-and-schema-validation)
  - [4.3 Continuous calibration and validation](#43-continuous-calibration-and-validation)
  - [4.4 Bounded instrumentation overhead](#44-bounded-instrumentation-overhead)
  - [4.5 Progressive scalability](#45-progressive-scalability)
- [5. Validity and review of assumptions](#5-validity-and-review-of-assumptions)


---

# Introduction

This document enumerates the primary assumptions that bound the conceptual design, future development and potential deployment of Vulture. The assumptions define the operational envelope in which architectural choices, detection mechanisms and ethical constraints are meaningful and defensible. They are not absolute guarantees; instead they describe reasonable preconditions under which the system can provide useful signals.

See related project documentation for context:

>Architecture: [[docs/architecture/architecture.md]]

>Threat model: [[docs/threat-model/threat-model.md]]

>Ethics: [[docs/ethics/ethics.md]]



---

# 1. Technical assumptions

## 1.1 Kernel-level instrumentation privileges

Instrumentation mechanisms (for example, eBPF-based collectors or equivalent kernel-level observers) are assumed to have the privileges necessary to observe the targeted kernel and user-space events. The design assumes these instrumentation paths are available and not purposely disabled or tampered with at observation time.

## 1.2 Relative integrity of telemetry

Telemetry captured via kernel- and agent-based instrumentation is assumed to be representative of the host’s behaviour. The system does not assume perfect integrity of every telemetry source; rather, it assumes an adversary does not simultaneously control or fully compromise all deployed observation points, allowing cross-source correlation to expose inconsistencies.

## 1.3 Constrained use of large language models (LLMs)

LLMs are used strictly as explanatory and augmentative components. LLM outputs must be reproducible, anchored to observable evidence, and fully auditable. LLMs do not act as final, automated decision-makers or as sole signals for alerts.

## 1.4 Local communication channels

Inter-component communication within the host (for example Unix domain sockets) is assumed to operate on trusted local channels under an explicit process-trust model. The architecture assumes those channels are available and not transparently proxied or intercepted by an adversary.

## 1.5 Linux kernel support and observability

Target environments provide a Linux kernel with functional support for the planned instrumentation stack (e.g., eBPF or equivalent hooks and tracing capabilities). The design assumes the kernel provides the minimum capabilities needed for the observability model to function.


---

# 2. Operational assumptions

## 2.1 Defined target environment

Vulture is intended for Linux systems where kernel-level observability is permitted and does not violate operational policy. Deployments are scoped to systems where such instrumentation is feasible and acceptable.

## 2.2 Human-in-the-loop operation

Skilled human operators are assumed to exist to review alerts, examine explanations and make final remediation decisions. Vulture is an assistive detection system, not an autonomous remediation engine.

## 2.3 Reasonable resource availability

Hosts or monitoring infrastructure are assumed to have sufficient CPU, memory and storage to support continuous observation, event processing and short-term retention of telemetry without causing unacceptable degradation of normal host operations.

## 2.4 Integration with existing processes

Vulture is designed to augment — not replace — established incident response, risk-management and compliance workflows. It provides signals, context and rationale rather than being the single source of truth for final actions.

## 2.5 Operator statistical literacy

Operators are assumed to understand the probabilistic and fallible nature of behavioural detection systems and interpret outputs accordingly (e.g., expected false positive/negative trade-offs).


---

# 3. Ethical assumptions

(See docs/ethics/ethics.md for the extended ethical framework.)

## 3.1 Defensive use only

Vulture is intended strictly for defensive research, security analysis and defensive operations. Use for offensive or privacy-invasive purposes is forbidden.

## 3.2 Not a surveillance system

The system is not designed for user surveillance, personnel evaluation, disciplinary measures, profiling or social control.

3.3 Data minimization and privacy protection

Data collection and processing shall follow minimization principles: collect only data required for detection, avoid unnecessary exposure of PII, and apply retention policies proportional to the investigative need.

## 3.4 Explainability & auditability requirement

Any alert, inference or classification must be traceable to concrete observations and reproducible processing steps. Opaque, non-auditable mechanisms that cannot be justified technically are incompatible with the project’s ethical baseline.


---

# 4. Implementation assumptions

## 4.1 Modular, hierarchical architecture

The hierarchical modular design enables incremental development and component replacement without invalidating the broader system. The architecture supports research-driven component evolution.

## 4.2 Structured data and schema validation

Telemetry normalization and use of explicit schemas (e.g., JSON Schema or equivalent) are assumed. Structured schemas enable validation, repeatable analysis and interoperability between components.

## 4.3 Continuous calibration and validation

Probabilistic models and behavioural detectors require ongoing calibration, validation and periodic re-evaluation using human feedback and technical review cycles.

## 4.4 Bounded instrumentation overhead

Instrumentation overhead (CPU, memory, latency) is assumed to be measurable and controllable. The project assumes overhead can be characterized and constrained within acceptable margins relative to host profile.

## 4.5 Progressive scalability

Storage and analysis components are assumed to scale progressively with telemetry volume; the design avoids forcing monolithic architectures in early stages and favors incremental scaling patterns.


---

# 5. Validity and review of assumptions

These assumptions are not static. Changes in Linux kernel behaviour, new adversarial evasion techniques, advances in observability, or empirical results from research may require assumption revision.

This document is normative but revisable. Assumptions should be re-evaluated routinely as part of the project’s maturity lifecycle and recorded alongside corresponding validation evidence (test results, benchmarks, incident retrospectives).







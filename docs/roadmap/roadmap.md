Project Vulture — Development Roadmap



**Status**: Planning & Research Phase
**Target Timeline**: 12 Months (Post-Funding)


---
# Overview

This roadmap outlines the strategic development of Project Vulture, transitioning from theoretical research to a functional Minimum Viable Product (MVP). The plan prioritizes the acquisition of necessary infrastructure, the establishment of a "white-box" telemetry architecture, and the ethical implementation of local Machine Learning models.

# Phase 1: Infrastructure & Telemetry Foundation
#### Timeline: Months 1–4
#### Focus: Hardware Setup, Telemetry Acquisition, Rule-Based Detection.

 * Infrastructure Procurement: Acquisition and configuration of the primary development workstation and laboratory hardware (per Funding Proposal budget).
 * Environment Setup: Deployment of isolated Linux environments for safe malware detonation and analysis.
 * Telemetry Backbone: Development of the initial data collection layer to capture system events with high granularity.
 * Basic Detection: Implementation of hierarchical, rule-based models to identify simple anomalous patterns without heavy computational reliance
 
# Phase 2: Architecture Validation & Interconnectivity
#### Timeline: Months 4–6
#### Focus: Simulation, Communication Protocols, Virtualization.

 * Inter-Model Communication: Definition and coding of Python-based protocols allowing distinct detection modules to exchange data.
 * Simulation Testing: Validating the architecture's logic within virtualized environments to ensure stability under stress.
 * Hierarchy Validation: Testing the hierarchical detection approach (from low-level telemetry to high-level analysis) to ensure no critical events are missed.
 
# Phase 3: Optimization & Data Integration

#### Timeline: Months 6–9
#### Focus: Latency Reduction, Data Composition, Resource Efficiency.

 * Performance Tuning: Refactoring code to minimize latency and computational overhead, ensuring the system runs efficiently on local infrastructure.
 * Telemetry Classification: Improving the tagging and sorting of collected data to reduce noise.
 * Integral Composition: Unifying disparate data streams into cohesive, human-readable event logs (adhering to the "transparency" principle).
# Phase 4: Advanced Intelligence & LLM Integration
#### Timeline: Months 9–12
#### Focus: Machine Learning, Local LLMs, Ethical Implementation.

 * ML Model Training: Training specific Machine Learning models on the curated telemetry data to detect complex, non-linear threat patterns.
 * LLM Monitoring: Integrating monitoring capabilities for local Large Language Model (LLM) logs to audit AI behavior.
 * Ethical Auditing: Reviewing all ML implementations against the project's ethical framework to ensure explainability and prevent "black-box" decision-making.
#### Phase 5: MVP Delivery & Documentation

#### Timeline: Month 12+

#### Focus: Final Prototyping, Metrics, Public Release.
 * Functional Prototype: Release of a fully virtualized, functional MVP capable of detection, correlation, and reporting.
 * Metric Analysis: Generation of reports demonstrating detection rates, false positive ratios, and system performance.
 * First-Class Documentation: Finalizing all technical documentation, including:
   * [[docs/architecture/Architecture]]
   * [[docs/ethics/ethics]]
   * [[research/assumptions]]

---

# Strategic Pillars

Throughout all phases, development is guided by three core pillars derived from the technical profile:

 * Transparency: Every alert must be explainable. The user must understand why a decision was made.
 * FOSS Philosophy: Dependencies and outputs will remain open-source to democratize defensive security.
 * Local-First: All processing, including AI/ML inference, is designed to run locally to preserve privacy and data sovereignty.
 ____
 *Date: 29/01/26 • Ernesto M. Alfonso Campos* 
# Vulture

A local defensive system that employs various machine learning components and rule-based models in a hierarchical manner to address threat contextualization in Linux systems using kernel tools.

---

# Summary

Vulture is a research project that explores the capabilities of hierarchical architectures in detection systems. It employs differentiation between levels and uses hierarchical components that form the foundation of the hierarchy, such as telemetry extractors and sanitizers.
The goal is to create a defensive system capable of contextual analysis and generating detailed reports using a language model.

---

# Motivation

Current detection mechanisms can often be opaque and lack the ability to perform contextual and/or historical analysis. This makes post-incident analysis difficult. Vulture is designed to allow semantic behavioral explanation and interpretation to assist humans in classifying threats and deploying protections.

---

# High-Level Approach

- Hierarchical Plugins: Support programs with key functions.
- Lower Level: Rule-based detection for behavioral index analysis.
- Middle Level: Temporal correlation based on logic and probability.
- Upper Level: Contextual reasoning.
- Human-in-the-Loop: No fully automated decisions; manual intervention is required for greater accuracy.

---

# Current Status

The project is in a research and conceptual development phase, prioritizing the selection of key technologies, proposing new architectural compositions, and suggesting improved development methods. There is no MVP due to constraints on the lead developer’s context.

---

# Roadmap

[✓] Create the repository with a logical structure for the documentation phase.
[✓] Complete initial versions of documentation on architecture, ethical approach, and assumptions.
[✓] Develop threat model documentation and development limitations.
[—] Apply for funding through NLnet (more information in: [[supplementary-documentation/funding-proposal.md]]).
[—] Begin laboratory development of an MVP with small models and limited telemetry sources.

---

# Contributing

To contribute voluntarily to the development of this software, an understanding of several key concepts is required:

- Organization: Essential for teamwork, as the system is variably composed and requires layered development.
- Mastery of Key Technologies:
  - Developing machine learning and artificial intelligence algorithms.
  - General knowledge of the Linux Kernel and its underlying technologies.
  - Experience with defensive tools and prior understanding of SOC (Security Operations Center) internal operations (the project's primary objective).
  - Proficiency in programming languages such as Rust and Python.

---

# Repository Structure

```
Vulture
├── AUTHORS.md
├── CONTRIBUTING.md
├── Checklist.md
├── README.md
├── docs
│   ├── architecture
│   │   └── Architecture.md
│   ├── design
│   ├── ethics
│   │   └── Ethics.md
│   ├── roadmap
│   │   └── roadmap.md
│   ├── technical-profile
│   │   └── Technical Profile.md
│   └── threat-model
│       └── threat-model.md
├── research
│   ├── assumptions.md
│   └── limitations.md
├── specs
│   └── report-schema.md
├── src
│   └── README.md
└── supplementary-documentation
    └── funding-proposal.md
```

**References:**

- Architecture: [[docs/architecture/Architecture.md]]
- Ethics: [[docs/ethics/Ethics.md]]
- Assumptions: [[research/assumptions.md]]
- Technical Profile: [[docs/technical-profile/Technical Profile.md]]
- Roadmap: [[docs/roadmap/roadmap.md]]

**Note**: The current structure corresponds to the research phase of the project. Changes may occur as the project progresses through different stages.

---

# Acknowledgments

Thank you for considering this project proposal. Any form of contribution is appreciated—whether advice, constructive criticism, code or technology suggestions, or commitment as a collaborator.

---

# License

This project is licensed under the [GPLv3](LICENSE.md) — see the LICENSE.md file for more information.

---

*Last updated: 27/01/26 – v1.0 (final draft)*
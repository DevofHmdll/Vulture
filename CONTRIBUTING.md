## Contributing to Project Vulture!!

Thank you for your interest in contributing to **Project Vulture**. This project is an open-source initiative dedicated to developing transparent, local-first threat detection for Linux environments.
To ensure system stability and auditability, we employ a hybrid development strategy: Rust for performance-critical tasks and Python for architectural logic and orchestration.
# 1. Core Philosophy

All contributions must align with the following principles:

**Explainability (White-Box)**: We do not accept "black-box" modules. Every detection and system action must be auditable and interpretable by a human operator.
**Local Sovereignty**: Features should prioritize local execution to maintain user privacy and data integrity.
**Safety & Performance**: Low-level system interactions must be memory-safe and resource-efficient.
# 2. Technical Stack & Standards

### Telemetry & Sanitization (Rust)
The core sensor and data-cleaning modules are built in Rust.
**Scope**: Kernel-level event extraction, high-speed data sanitization, and normalization.
Requirements:
Code must be formatted using cargo fmt.
Submissions must pass cargo clippy without warnings.
The use of unsafe blocks is discouraged; if required for FFI or kernel interaction, they must be rigorously documented with safety justifications.
Architecture & Logic Prototyping (Python)
The higher-level detection logic, inter-model communication, and ML integrations are prototyped in Python.
Scope: System orchestration, behavioral simulation, and LLM log monitoring.
Requirements:
Strict adherence to PEP 8.
Type Hinting is mandatory for all architectural components to ensure long-term maintainability.
Comprehensive docstrings explaining the underlying detection logic for each module.
3. How to Contribute
Documentation First
We treat documentation as a first-class artifact.
Before submitting code, ensure that related documents in docs/architecture/ or research/assumptions/ are updated to reflect your changes.
PRs that significantly alter system behavior without accompanying documentation will not be merged.
Pull Request Process
Check the Roadmap: Ensure your contribution aligns with the current phase in ROADMAP.md.
Branching: Use descriptive branch names (e.g., feat/rust-event-collector or refactor/python-logic-flow).
Testing: Provide proof of stability (e.g., logs from a virtualized testing environment).
Review: All PRs will be reviewed for both technical efficiency and adherence to the project’s Ethical Framework (docs/ethics/).
4. Licensing
By contributing to Project Vulture, you agree that your work will be licensed under the GNU General Public License v3 (GPLv3).
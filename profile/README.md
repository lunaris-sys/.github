# Lunaris

A Linux-based desktop operating system built around a system-wide Knowledge Graph as first-class infrastructure.

Every file access, window focus, application event, and user action is recorded as typed graph nodes and edges, queryable across application boundaries and time. The AI layer, the permission model, the project system, and the shell are all built on top of this graph rather than operating in isolation.

The full architecture is documented in the [Foundation blueprint](https://github.com/lunaris-sys/foundation):

> Kicker, T. (2026). *A Capability-Based, Event-Driven Linux Desktop: Knowledge Graph Architecture and Design Rationale.* Zenodo. https://doi.org/10.5281/zenodo.19119730

The blueprint covers the complete system stack: eBPF event pipeline, Knowledge Graph (SQLite + Ladybug), AI layer, Wayland compositor fork, shell (Tauri + TypeScript), capability-based permission model, security threat model, and implementation roadmap.

*Contributions and architecture discussions welcome. Open an issue in the foundation repo.*

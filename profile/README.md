# Arlen

A capability-based, event-driven Linux desktop, built from scratch. It is in early development (pre-alpha), so things move and break often.

The system is organised around three ideas. A system-wide knowledge graph that the whole desktop reads and writes, treated as real infrastructure rather than a per-app database, so the system has context: which files belong to a project, what you worked on last week, which applications talk to each other. Capability tokens for every cross-component access, so a component only ever reaches what it was granted. And a modern shell that replaces the usual Linux desktop pieces, meant to be usable by non-technical people, with no telemetry and no lock-in.

## Foundation

The full architecture is a published technical report:

> Kicker, T. (2026). *A Capability-Based, Event-Driven Linux Desktop: Knowledge Graph Architecture and Design Rationale.* Zenodo. https://doi.org/10.5281/zenodo.19119730

It covers the whole stack: the eBPF event pipeline, the knowledge graph, the AI layer, the Wayland compositor, the shell, the capability-based permission model, the security threat model, and the roadmap. It is the ground truth for design decisions.

## Where the code lives

Most of the first-party code is one monorepo, [`arlen`](https://github.com/arlenos/arlen), because the layers are tightly coupled and change together. A couple of things keep their own repos where independence matters: the [`compositor`](https://github.com/arlenos/compositor) (a fork of [cosmic-comp](https://github.com/pop-os/cosmic-comp) that tracks upstream) and the [`foundation`](https://github.com/arlenos/foundation) paper. The monorepo is laid out as `contracts/` (shared wire crates), `daemons/`, `ai/`, `sdk/`, `apps/`, `forage/`, `themes/`, and `distro/`, with architecture specs under `docs/`.

## The stack, bottom to top

A kernel layer normalises eBPF tracepoints into events. An event bus routes those events between components over Unix sockets. The knowledge daemon writes them into a graph (SQLite on the write side, a graph engine for queries) and adds the project and timeline features on top. The SDK gives first-party apps typed access to the graph, the bus, configuration, permissions, and theming.

A set of daemons fills out the system: an append-only audit ledger, an advisory anomaly detector that informs and never blocks, a D-Bus notification server, a module runtime that sandboxes third-party extensions, and an install daemon for packages, modules, and Flatpaks. The AI layer sits above the graph as a capability-gated agent: every action runs predict, gate, act, audit, reads are scoped to a configured tier, external content is screened for prompt injection and parsed in a locked-down sandbox, and interactive queries reach the graph and tools over a read-only MCP boundary.

The compositor is a cosmic-comp fork wired into the event bus, and the shell (top bar, launcher, notifications, settings, the AI harness) is built with Tauri 2 and Svelte 5 on a shared component library, so UI is never copied between apps.

## Status

Pre-alpha. The daemons, the SDK, the shell, and the AI layer exist and build, and large parts are covered by tests, but this is not a daily driver yet, and breaking changes land without migration shims.

## Stack

Rust throughout the system layer. Tauri 2, SvelteKit, Svelte 5, TypeScript, Tailwind, and shadcn-svelte for the UI. A Wayland session on the cosmic-comp fork.

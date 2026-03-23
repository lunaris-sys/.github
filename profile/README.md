# Lunaris

A Linux desktop OS built around a system-wide Knowledge Graph as first-class infrastructure.

The graph gives the system context: which files belong to a project, what you were working on last Tuesday, which applications interact with each other. The AI layer, permission model, project system, and shell are all built on top of this shared context rather than operating in isolation.

## Design

The full architecture is documented in the Foundation blueprint:

> Kicker, T. (2026). *A Capability-Based, Event-Driven Linux Desktop: Knowledge Graph Architecture and Design Rationale.* Zenodo. https://doi.org/10.5281/zenodo.19119730

The blueprint covers the complete system stack: eBPF event pipeline, Knowledge Graph, AI layer, Wayland compositor, shell architecture, capability-based permission model, security threat model, and implementation roadmap.

## What's being built

The system is organized as a set of independent repositories under this organization:

| Repo | What it is |
|------|-----------|
| `event-bus` | Unix socket event bus with protobuf encoding and subscription routing |
| `knowledge` | SQLite write store + Ladybug graph query layer + graph daemon |
| `os-sdk` | Rust + TypeScript SDK for first-party applications |
| `kernel-layer` | eBPF normalizer (aya) - captures file access events from the kernel |
| `compositor` | Wayland compositor fork of [cosmic-comp](https://github.com/pop-os/cosmic-comp) with Lunaris event integration |
| `desktop-shell` | Shell UI: Top Bar, Waypointer launcher, Notifications - built with Tauri + Svelte + shadcn-svelte |
| `ui-kit` | Shared component library for first-party apps - Shadcn design system, Surface Tokens, live theming |
| `themes` | Theme system: TOML token format, GTK4 generator, live switching |
| `distro` | Dev environment, VM setup, build tooling |

## Current status

The data pipeline is complete and end-to-end tested: eBPF events flow through the kernel layer into the event bus, land in SQLite, and are queryable via the graph daemon. The desktop shell renders a live Top Bar that reacts to compositor window focus events in real time. The Shadcn-based design system is operational with live theme switching.

Active work is on the compositor layer: xdg-decoration CSD negotiation is done, wlr-foreign-toplevel-management is next.


## Stack

Rust throughout the system layer. Tauri + Svelte 5 + TypeScript + Tailwind v4 + shadcn-svelte for all UI. Fedora as the base distribution. cosmic-comp as the compositor foundation.


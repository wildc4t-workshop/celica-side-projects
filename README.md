# Celica Side Projects

Optional engineering projects for the 2000 Toyota Celica GT-S that are **not required** to complete either Celica Baseline or the committed Street Build.

This repository is intentionally a compact recovery/execution space rather than a collection of separate repositories. At present, only two side projects are active enough to deserve dashboard work: **Big Brake Kit** and **EPS**.

## Active Project Index

### Big Brake Kit — verification / design completion

Last authoritative checkpoint:

- Front selected direction: **Wilwood Superlite + Corvette rotor redrilled to 5x100**.
- A 3D-printed front adapter was produced and physically fit-checked successfully.
- The metal front adapter was not released because the structural FEA inputs/load cases were not yet considered defensible.
- Rear geometry using an **LS430 rear caliper + redrilled Nissan rotor** was technically viable in CAD, but required more custom hub/adapter machining than desired and was never accepted as the final rear solution.
- The Superlite/LS430 combination was selected in part because prior calculations indicated an appropriate front/rear brake balance; those calculations still need to be recovered or independently verified.
- A larger-bore Highlander master cylinder was being investigated, but the exact application and bore remain to be verified.

See [`bbk/PROJECT.md`](bbk/PROJECT.md) and [`bbk/SOURCES.md`](bbk/SOURCES.md).

### EPS — architecture recovery / mechanical interface design

The EPS project is feasible in principle but stalled on creating a clean, repeatable steering-column interface.

Known state:

- Multiple Nissan Versa EPS components are available for investigation.
- An MR2 Spyder electro-hydraulic pump/reservoir assembly is available as a lower-risk fallback architecture.
- Spare Celica steering hardware exists, including at least one spare column.
- The original blocker was adapting donor EPS input/output shaft geometry to the Celica without spline tooling and without committing to uncontrolled one-off cut/weld fabrication.
- A spare column should not be cut until the complete interface, mounting, and load path are defined in CAD.

The engineering target is a **drawing-controlled, inspectable, reproducible mechanical interface** using OEM splines, commercial couplers/U-joints, bolted/clamped interfaces, or properly machined adapters wherever practical.

See [`eps/PROJECT.md`](eps/PROJECT.md) and [`eps/SOURCES.md`](eps/SOURCES.md).

## Parked Concepts

Other ideas such as AWD conversion, tubular subframe development, and small external projects are intentionally **not represented in `tasks.csv`**. They remain concepts until deliberately revived; they should not clutter the dashboard or compete with real work.

## Source of Truth

- [`tasks.csv`](tasks.csv) — canonical executable work queue and task status for BBK/EPS.
- [`project.yaml`](project.yaml) — machine-readable project state.
- [`AGENTS.md`](AGENTS.md) — collaboration, DFM, safety, and repository-maintenance rules.
- [`bbk/PROJECT.md`](bbk/PROJECT.md) — authoritative BBK resume-here state.
- [`eps/PROJECT.md`](eps/PROJECT.md) — authoritative EPS resume-here state.

## Rules

- Ownership of hardware does **not** imply architectural commitment.
- Explored concepts remain explored until explicitly selected.
- Side projects do not get to redefine Baseline or Street Build completion.
- Prefer a concise restart document over empty project-management structure.
- `tasks.csv` contains only work that is useful to resume, verify, or advance BBK or EPS.
- For safety-critical steering/brake hardware, prototype fit is not design release; structural/mechanical verification comes first.

## Dashboard

This repository exposes `project.yaml` and `tasks.csv` to the Celica Project Dashboard.

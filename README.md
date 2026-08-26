# Celica Side Projects

Optional engineering projects and experiments for the 2000 Toyota Celica GT-S that are **not required** to complete either Celica Baseline or the committed Street Build.

This repository is intentionally an index/recovery space rather than a collection of separate repositories. A side project gets its own folder when enough real work exists to preserve a useful **resume-here** state.

## Project Index

### Big Brake Kit — active recovery / verification

The BBK is the most mature side project currently reconstructed.

Last authoritative checkpoint:

- Front selected direction: **Wilwood Superlite + Corvette rotor redrilled to 5x100**.
- A 3D-printed front adapter was produced and physically fit-checked successfully.
- The metal front adapter was not released because the structural FEA inputs/load cases were not yet considered defensible.
- Rear geometry using an **LS430 rear caliper + redrilled Nissan rotor** was technically viable in CAD, but required more custom hub/adapter machining than desired and was never accepted as the final rear solution.
- The Superlite/LS430 combination was selected in part because prior calculations indicated an appropriate front/rear brake balance; those calculations still need to be recovered or independently verified.
- A larger-bore Highlander master cylinder was being investigated, but the exact application and bore remain to be verified.

See [`bbk/PROJECT.md`](bbk/PROJECT.md) for the full restart state and [`bbk/SOURCES.md`](bbk/SOURCES.md) for the earlier documented design history.

### EPS — not yet reconstructed

Real hardware and prior work exist, but the current architecture/progress has not yet been recovered into this repository.

### Tubular subframe — not yet reconstructed

Early engineering/learning project. Preserve later when there is enough state to create a useful restart document.

### AWD concept — not yet reconstructed

Long-term concept investigation, not a hidden requirement for the Street Build.

### M-Gauge — saved external mini-project

Interesting project saved for possible future use; no active Celica integration work is currently defined here.

## Rules

- Ownership of hardware does **not** imply architectural commitment.
- Explored concepts remain explored until explicitly selected.
- Side projects do not get to redefine Baseline or Street Build completion.
- Prefer a concise restart document over empty project-management structure.
- `tasks.csv` contains only work that is useful to resume, verify, or advance a side project.

## Dashboard

This repository exposes [`project.yaml`](project.yaml) and [`tasks.csv`](tasks.csv) to the Celica Project Dashboard.

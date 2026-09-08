# Celica Side Projects

Optional engineering projects for the 2000 Toyota Celica GT-S that are **not required** to complete either Celica Baseline or the committed Street Build.

This repository is intentionally a compact recovery/execution space rather than a collection of separate repositories. At present, only two side projects are active enough to deserve execution work: **Big Brake Kit** and **EPS**. The P4 e-AWD concept is preserved here as a **parked engineering side project** so it can be resumed later without reconstructing the design from memory; it carries only one passive donor-watch task.

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

## Parked Engineering Side Project

### P4 e-AWD / Hybrid Rear Axle — Y61 architecture selected, passive donor watch only

The current checkpoint uses a **Mitsubishi Y61 rear motor/reduction/differential**, its **matched OEM Mitsubishi rear inverter**, and a compact **Ford C-Max Hybrid non-Energi high-power battery**.

Current direction:

- retain the turbo 2ZZ + E153 FWD drivetrain as the primary propulsion system;
- add an independent electric rear axle for launch traction, torque fill, shift fill, corner-exit front-tire relief, low-mu traction, regen and optional through-the-road charging;
- preserve the OEM fuel tank if packaging allows;
- use the existing Matrix AWD rear assembly later as a suspension/hard-point geometry donor rather than assuming direct subframe interchange;
- retain the Y61's stock 7.065:1 gearing because the launch multiplication is valuable;
- treat the Y61 as roughly a ~70 kW / 195 Nm / ~950-lbf-at-the-tire rear system on the assumed 235/35R18 tire;
- use the Mitsubishi OEM rear inverter because documented standalone CAN torque control exists, avoiding a separate custom PMSM-control development program;
- retain the Ford BECM/contactors/cooling if a minimal standalone CAN contract can be decoded;
- develop a staged one-side inboard halfshaft disconnect so aggressive stock gearing does not mechanically overspeed the rear motor at high road speed;
- software-limit the car to approximately **90 mph until the disconnect is validated**;
- develop the entire system off-car and integrate only after packaging, inverter CAN, battery CAN, HV safety and disconnect behavior are proven.

Intentional trade:

- the Y61 is materially heavier than the Toyota Q610 (~133 lb community eAxle benchmark versus ~91 lb), but it buys more usable 25–60 mph rear power and substantially lower motor-control R&D burden;
- Q610 remains the first lightweight alternative to revisit if turnkey inexpensive standalone torque/regen control matures or Y61 packaging/mass fails;
- the earlier Q211 + Gen-3 Prius-inverter + custom ~4.07 regear direction is superseded because the regear solved overspeed by sacrificing too much low-speed rear thrust while adding custom gear-design/manufacturing burden.

Current planning gates:

- finished DIY cost preferably around **$6k**, with roughly **$5k–$7k** as a planning band;
- net mass addition preferably **<= ~275 lb**, investigate up to roughly **300 lb** before rejecting Y61;
- OEM fuel tank retained if practical;
- no unrestricted high-speed/track operation before disconnect validation.

**Near-term rule:** `SIDE-EAWD-001` remains a passive sourcing watch only. Prefer complete same-donor Y61 + rear-inverter packages with HV cables, pigtails, brackets and useful CV hardware. A mechanically failed but electrically healthy C-Max Hybrid remains the preferred battery/CAN reference donor. Do not create additional e-AWD execution work until the project is explicitly revived.

See:

- [`eawd/PROJECT.md`](eawd/PROJECT.md) — authoritative resume-here checkpoint;
- [`eawd/CURRENT_ARCHITECTURE.md`](eawd/CURRENT_ARCHITECTURE.md) — concise current architecture;
- [`eawd/TRADE_STUDY_2026-09-08.md`](eawd/TRADE_STUDY_2026-09-08.md) — calculations, Q610/Q211 tradeoffs, disconnect rationale and vehicle-dynamics benefits;
- [`eawd/SOURCES_CURRENT.md`](eawd/SOURCES_CURRENT.md) — current Y61/inverter/C-Max evidence;
- [`eawd/SOURCES.md`](eawd/SOURCES.md) — retained legacy Q211/Prius research.

## Other Parked Concepts

Other ideas such as tubular-subframe development and small external projects are intentionally **not represented in `tasks.csv`**. They remain concepts until deliberately revived; they should not clutter the dashboard or compete with real work.

## Source of Truth

- [`tasks.csv`](tasks.csv) — canonical executable work queue and task status for BBK/EPS plus the single passive e-AWD donor-watch task.
- [`project.yaml`](project.yaml) — machine-readable project state.
- [`AGENTS.md`](AGENTS.md) — collaboration, DFM, safety, and repository-maintenance rules.
- [`bbk/PROJECT.md`](bbk/PROJECT.md) — authoritative BBK resume-here state.
- [`eps/PROJECT.md`](eps/PROJECT.md) — authoritative EPS resume-here state.
- [`eawd/PROJECT.md`](eawd/PROJECT.md) — durable parked P4 e-AWD current checkpoint.

## Rules

- Ownership of hardware does **not** imply architectural commitment.
- Explored concepts remain explored until explicitly selected.
- Side projects do not get to redefine Baseline or Street Build completion.
- Prefer a concise restart document over empty project-management structure.
- `tasks.csv` contains only work that is genuinely active enough to deserve engineering attention, plus explicitly authorized passive sourcing/watch items.
- Parked projects may have detailed Markdown documentation without being promoted into execution work.
- For safety-critical steering/brake/high-voltage hardware, prototype fit is not design release; structural, electrical, thermal, controls, and fault verification come first.

## Dashboard

This repository exposes `project.yaml` and `tasks.csv` to the Celica Project Dashboard. Parked e-AWD documentation remains parked; only its single passive donor-watch task is exposed.

# AGENTS.md — Celica Side Projects

## Mission

This repository is the engineering system of record for optional Celica engineering projects that are real enough to preserve and advance, but are **not required** for Celica Baseline or the committed Street Build.

At present, only two side projects are active enough to generate normal execution work:

- DIY Big Brake Kit (BBK)
- Electric Power Steering (EPS)

The P4 e-AWD / hybrid rear-axle project is a **documented parked side project**. It has enough engineering content to deserve durable Markdown and one explicitly authorized passive donor-watch task (`SIDE-EAWD-001`), but it should not generate additional execution work until explicitly revived.

## Core operating rule

**Markdown is durable engineering memory. `tasks.csv` is engineering attention. `project.yaml` is machine-readable state. The dashboard is derived only.**

Do not leave safety-critical design rationale only in chat, CAD, screenshots, marketplace notes, or task rows.

A parked project may have substantial durable documentation and a passive sourcing task without being promoted into active execution work.

## Read before changing state

Read at minimum:

- `README.md`
- `project.yaml`
- `tasks.csv`
- `bbk/PROJECT.md` and `bbk/SOURCES.md` for BBK work
- `eps/PROJECT.md` and `eps/SOURCES.md` for EPS work
- `eawd/PROJECT.md`, `eawd/CURRENT_ARCHITECTURE.md`, `eawd/TRADE_STUDY_2026-09-08.md`, and `eawd/SOURCES_CURRENT.md` for P4 e-AWD work

Treat current repo state as authoritative unless the user explicitly corrects it.

`eawd/SOURCES.md` is retained legacy Q211/Prius research and does **not** define the current rear-drive architecture.

## Collaboration rule

The user may report natural-language updates from any chat. Do not require task IDs or filenames. Resolve the affected state, update durable documentation/tasks when appropriate, and report what changed.

If the user says not to update GitHub yet, discuss only.

## Task and decision IDs

Use consolidated side-project IDs:

- `SIDE-BBK-###`
- `SIDE-EPS-###`
- `SIDE-EAWD-001` is reserved for the parked project's passive donor-watch task
- additional `SIDE-EAWD-###` tasks only after the e-AWD project is explicitly promoted out of parked state

Use decision IDs:

- `DEC-SIDE-BBK-###`
- `DEC-SIDE-EPS-###`
- `DEC-SIDE-EAWD-###`

Canonical task schema:

```text
id,title,status,action,time_min,context,cost,priority,blocked_by,decision_needed,doc_link,requires_car_down,requires_parts,notes
```

## State / evidence discipline

Classify conclusions as fact/observation, inference, tentative direction, selected decision, rejected/superseded decision, open question, or task.

Useful evidence labels include `MEASURED`, `FIT-CHECKED`, `BENCH-TESTED`, `MANUFACTURER`, `FACTORY-DOC`, `CAD-DERIVED`, `CALCULATED`, `INFERRED`, and `TENTATIVE`.

**Ownership of hardware does not imply architectural commitment. Prototype fit does not equal design release.**

For safety-critical brake, steering, high-voltage, drivetrain, and suspension hardware, preserve exact geometry, material, fastener, load-case, controls, thermal, and verification assumptions as applicable.

# Big Brake Kit

## Authoritative checkpoint

Front selected architecture:

- Wilwood Superlite caliper;
- Corvette front rotor redrilled to 5x100;
- custom Celica-to-Superlite adapter;
- 3D-printed adapter physically fit-checked successfully;
- permanent metal adapter release held pending defensible structural analysis.

Rear:

- LS430 rear caliper + redrilled Nissan rotor is a technically viable known candidate;
- it was not accepted as final because it required more custom/irreversible hub and adapter work than desired;
- treat it as the known-valid baseline candidate, not a failed concept and not a frozen final design.

System-level:

- prior calculations suggested the Superlite-front / LS430-rear combination maintained appropriate front/rear brake balance;
- those calculations must be recovered or independently recomputed;
- a larger-bore Highlander master-cylinder candidate was being investigated, but exact application/bore and pedal effects remain to be verified.

## BBK verification rules

Derive bracket loads from the actual brake system. Use bounded values for line pressure, piston area, pad friction, effective rotor radius, mount geometry, material/thickness, bolt grades and thread engagement.

Verify more than peak von Mises stress. Evaluate bearing, net section/tear-out, edge distance, bracket deflection, bolt interaction, thread engagement, contact/slip and fatigue sensitivity as applicable.

Separate brake-torque distribution from pedal feel/master-cylinder sizing. Do not release the permanent front adapter until structural and hydraulic/system-level implications are documented.

Do not restart the rear design from a blank sheet. Compare alternatives against the known-valid LS430/Nissan architecture and require a meaningful serviceability/manufacturability improvement.

# Electric Power Steering

## Authoritative checkpoint

Available hardware includes multiple Nissan Versa column-EPS components, an MR2 Spyder electro-hydraulic pump/reservoir assembly, spare Celica steering hardware, and at least one spare Celica column.

The EPS project stalled because there was no clean, drawing-controlled mechanical integration plan between donor EPS shaft/spline geometry and the Celica steering column/intermediate shaft.

The user does not want a one-off `cut here / weld there` solution merely because it can be made to turn.

## EPS governing rule

> Do not cut a spare Celica column until the complete mechanical load path, shaft-interface strategy, mounting scheme, and packaging are defined.

Prefer OEM spline interfaces, commercial couplers/U-joints, useful OEM shaft segments, bolted/clamped/keyed interfaces, machined reproducible adapters, reversible prototype work and complete CAD before destructive modification.

Avoid arbitrary shaft cuts, hand-ground D-shafts, undocumented sleeve/weld splices and geometry dependent on one fabricator's fit-up.

A controlled welded component is not categorically forbidden if later engineering shows it is the best solution, but `cut and weld until it fits` is not an acceptable starting design.

### Column-mounted EPS

Nissan Versa hardware is the active R&D candidate. Solve spline/interface identification, axial packaging, motor clearance, steering-wheel position, shaft/U-joint geometry, housing reaction-torque mounting, collapse/telescoping behavior and torque/fatigue capacity before vehicle installation.

### MR2 Spyder electro-hydraulic fallback

Retain the Celica hydraulic rack and replace the engine-driven pump with the electric MR2 pump/reservoir assembly. This avoids column spline adaptation but retains hydraulic lines/fluid and requires pump packaging, line adaptation and control.

## EPS decision gate

> Can column EPS be integrated with controlled, reproducible mechanical interfaces that meet the project's DFM and safety standard without unacceptable Celica-column modification?

If yes, continue column EPS. If no, the MR2 electro-hydraulic architecture is a valid fallback.

# P4 e-AWD / Hybrid Rear Axle

## Authoritative checkpoint

This project is **parked**, but its current architecture is deliberately preserved because enough engineering work has been done that losing the rationale would create substantial rework later.

Current selected/tentative baseline:

- Mitsubishi **Y61** rear motor/reduction/open differential from a later Outlander PHEV;
- matched **OEM Mitsubishi rear traction inverter** using documented standalone CAN torque-control precedent;
- Ford **C-Max Hybrid non-Energi** compact ~281 V / 1.4-kWh high-power battery reference;
- preserve Ford BECM/contactors/cooling if standalone CAN control is practical;
- dedicated Celica VCU translating EMU/chassis/BMS state into requested rear motor torque;
- custom/modified rear cradle using Matrix AWD geometry as a hard-point reference;
- preserve OEM fuel tank if practical;
- retain Y61 stock **7.065:1** gearing for launch/mid-speed performance;
- future one-side inboard halfshaft disconnect using the open differential;
- approximately **90 mph temporary vehicle-speed limit until disconnect validation**;
- development off-car first, vehicle integration last.

Current planning gates:

- complete installed mass preferably <= ~275 lb; investigate to ~300 lb before rejecting Y61;
- finished DIY cost preferably around ~$6k, with ~$5k–$7k planning band;
- do not enlarge the battery merely to justify the Y61's 70 kW motor rating.

## e-AWD governing rule

> Use the lightest practical system that consumes otherwise-unused tire traction, but do not volunteer to reproduce mature OEM motor-control work when a matched inverter is already commandable.

The current Y61 decision intentionally accepts roughly ~40 lb more eAxle mass than Q610 because it buys:

- approximately ~950 lbf rear launch thrust on the assumed 235/35R18 tire;
- ~70 kW motor capability and stronger 25–60 mph force persistence;
- OEM resolver/current/field-weakening/motor-protection control;
- existing OEM-inverter CAN torque/regen precedent;
- less diversion into custom PMSM control.

## Alternative status

### Q610 — HOLD / watch list

Q610 remains the first lightweight alternative if:

- inexpensive documented standalone torque/regen control becomes turnkey; or
- Y61 packaging/mass fails.

Do not start a Gen-3 Prius/OpenInverter Q610 development program merely because the ~91-lb axle is attractive unless the user explicitly chooses that R&D burden.

### Q211 — SUPERSEDED final-drive direction

The prior Q211 + Gen-3 Prius inverter + custom ~4.07 regear architecture is superseded. The custom regear controlled overspeed but reduced calculated rear thrust too far for the now-defined traction mission while creating custom-helical-gear manufacturing/validation work.

Legacy Q211/OpenInverter sources remain in `eawd/SOURCES.md` for future reference.

## e-AWD battery / CAN discipline

The desired Ford strategy is to preserve OEM BECM functions for cell voltage, temperature, current, power limits, cooling, precharge/contactors and faults.

DOE-tested C-Max packs show meaningful unit-to-unit/test variation; do not design to the best published pulse-power number. The actual donor BECM's real-time power limits are authoritative.

Reverse-engineer only the message contract needed to operate the BECM standalone. Prefer an intact, electrically healthy but mechanically failed C-Max Hybrid as a future CAN/reference donor if one can be bought cheaply.

Do not bypass BECM limits to obtain rear torque.

## e-AWD speed / disconnect discipline

Stock Y61 gearing is deliberately retained. Until the actual donor speed limit and a mechanical disconnect are validated, keep the car software-limited to approximately 90 mph.

Preferred disconnect concept:

- one-side inboard shaft;
- bearing-supported dog clutch;
- manual/stationary first;
- positive engaged/disengaged sensing;
- electrical actuation later;
- motor-synchronized reconnection only after low-speed validation;
- rear torque = zero whenever disconnect state is unresolved.

Before relying on the open-differential disconnect, calculate and validate internal side-gear/pinion relative speed, lubrication and durability. Protecting the motor from overspeed does not automatically prove the differential is safe.

## e-AWD vehicle-dynamics discipline

The first vehicle POC is straight-line and conservative. Later add launch, shift-fill, regen, corner-exit front-tire relief, low-mu traction allocation and yaw-aware torque reduction one function at a time.

The Y61/open-diff architecture is **front/rear torque allocation**, not true left/right torque vectoring.

The VCU remains the sole torque authority. Driver inputs such as a regen button are requests, never direct inverter commands.

## e-AWD high-voltage discipline

Traction-battery and inverter work involves lethal voltage and stored energy. Preserve or intentionally replace OEM safety functions with equivalent engineered protection, including as applicable:

- service disconnect;
- precharge;
- main contactors;
- correctly rated fusing;
- HV interlock strategy;
- insulation/creepage/clearance;
- guarded terminals/connectors;
- de-energization verification;
- fault shutdown;
- battery/inverter/motor temperature limits;
- physical protection of HV cable routes.

Do not conduct high-power unloaded motor testing on an improvised fixture.

## e-AWD promotion gate

Do not add additional `SIDE-EAWD-*` tasks beyond `SIDE-EAWD-001` until the user explicitly revives the project.

The first meaningful gate after revival is:

> Can the Y61 be integrated into the Celica rear geometry, using the Matrix assembly as a suspension/hard-point reference, while preserving acceptable fuel-tank/floor/exhaust packaging, axle geometry, and a credible <=~300-lb complete-system mass?

If no, reconsider Q610 or keep the project parked without escalating sunk cost.

If yes, the next gate is low-energy OEM-inverter CAN bench control, followed by C-Max BECM work.

## Cross-project boundaries

- Baseline owns conventional hydraulic PS now.
- EPS is optional R&D and does not block Baseline.
- BBK and EPS do not redefine Street Build completion unless deliberately adopted later.
- P4 e-AWD is parked and does not redefine Street Build completion, rear-suspension architecture, fuel-system architecture, or current harness work unless explicitly adopted later.
- A future custom rear cradle may interact with suspension, exhaust, fuel-tank, battery and wiring work; record explicit interfaces rather than silently changing other repositories.

Create explicit cross-project dependencies rather than duplicate tasks.

## Definition of done

Before marking active BBK/EPS work done, or future e-AWD work if promoted:

1. preserve the useful result in the relevant durable document;
2. record exact parts/geometry/analysis assumptions where relevant;
3. update decisions/current architecture;
4. create only genuinely actionable follow-ons;
5. reconcile blocked/ready tasks;
6. preserve CAD/FEA/test/source references needed to reproduce the work.

For parked-project research, update durable Markdown without creating attention/task debt unless the user explicitly wants the project active. `SIDE-EAWD-001` may remain open indefinitely as a passive sourcing reminder.

## End-of-session reconciliation

After meaningful work ask:

- What exact interface or fact did we establish?
- What remains assumed?
- Did a design become selected, rejected, or merely more plausible?
- What verification is still required before fabrication/installation?
- What task state changed?
- Is the record sufficient to resume without reconstructing the problem from memory?

Do not preserve chat transcripts as project documentation. Convert the useful outcome into concise engineering state.

# AGENTS.md — Celica Side Projects

## Mission

This repository is the engineering system of record for optional Celica engineering projects that are real enough to preserve and advance, but are **not required** for Celica Baseline or the committed Street Build.

At present, only two side projects are active enough to generate dashboard work:

- DIY Big Brake Kit (BBK)
- Electric Power Steering (EPS)

The P4 e-AWD / hybrid rear-axle project is a **documented parked side project**. It has enough engineering content to deserve durable Markdown, but it intentionally does not generate dashboard work until explicitly revived.

Other ideas remain parked concepts until deliberately revived. Do not create tasks for them merely because they have been discussed before.

## Core operating rule

**Markdown is durable engineering memory. `tasks.csv` is engineering attention. `project.yaml` is machine-readable state. The dashboard is derived only.**

Do not leave safety-critical design rationale only in chat, CAD, FEA screenshots, marketplace notes, or task rows.

A parked project may have substantial durable documentation without being promoted into `tasks.csv`.

## Read before changing state

Read at minimum:

- `README.md`
- `project.yaml`
- `tasks.csv`
- `bbk/PROJECT.md` and `bbk/SOURCES.md` for BBK work
- `eps/PROJECT.md` and `eps/SOURCES.md` for EPS work
- `eawd/PROJECT.md` and `eawd/SOURCES.md` for P4 e-AWD work

Treat current repo state as authoritative unless the user explicitly corrects it.

## Collaboration rule

The user may report natural-language updates from any chat, for example:

- `BBK: I found the final adapter CAD.`
- `EPS: the Versa unit is a 2014 part.`
- `e-AWD: I pulled the Q211.`
- `The rear rotor is actually from a different Nissan.`

Do not require task IDs or filenames. Resolve the affected state, update durable documentation/tasks when appropriate, and report what changed.

If the user says not to update GitHub yet, discuss only.

## Task and decision IDs

Use consolidated side-project IDs:

- `SIDE-BBK-###`
- `SIDE-EPS-###`
- `SIDE-EAWD-###` only after the e-AWD project is explicitly promoted out of parked state

Use decision IDs:

- `DEC-SIDE-BBK-###`
- `DEC-SIDE-EPS-###`
- `DEC-SIDE-EAWD-###` for durable e-AWD architecture decisions if/when formal decision records are needed

Canonical task schema:

```text
id,title,status,action,time_min,context,cost,priority,blocked_by,decision_needed,doc_link,requires_car_down,requires_parts,notes
```

Do not create dashboard work for e-AWD, tubular-subframe, M-Gauge, or other parked ideas unless the user explicitly revives them.

## State / evidence discipline

Classify conclusions as fact/observation, inference, tentative direction, selected decision, rejected/superseded decision, open question, or task.

Useful evidence labels include `MEASURED`, `FIT-CHECKED`, `BENCH-TESTED`, `MANUFACTURER`, `FACTORY-DOC`, `CAD-DERIVED`, `CALCULATED`, `INFERRED`, and `TENTATIVE`.

**Ownership of hardware does not imply architectural commitment. Prototype fit does not equal design release.**

For safety-critical brake, steering, high-voltage, drivetrain, and suspension hardware, preserve exact geometry, material, fastener, load-case, manufacturing, controls, thermal, and verification assumptions as applicable.

# Big Brake Kit

## Authoritative checkpoint

Front selected architecture:

- Wilwood Superlite caliper
- Corvette front rotor redrilled to 5x100
- custom Celica-to-Superlite adapter
- a 3D-printed adapter was produced and physically fit-checked successfully
- permanent metal adapter release was intentionally held pending defensible structural analysis

Rear:

- LS430 rear caliper + redrilled Nissan rotor is a **technically viable** known candidate
- CAD existed and the geometry worked conceptually
- the solution was not accepted as final because it required more custom/irreversible hub and adapter work than desired
- treat it as the known-valid baseline candidate, not a failed concept and not a frozen final design

System-level:

- prior calculations suggested the Superlite-front / LS430-rear combination maintained an appropriate front/rear brake balance
- those calculations must be recovered or independently recomputed
- a larger-bore Highlander master-cylinder candidate was being investigated, but exact application/bore and pedal effects remain to be verified

## BBK analysis discipline

For the front adapter, derive local load from the actual brake system rather than inventing an arbitrary vehicle-g bracket force.

Use exact or bounded values for:

- line pressure
- caliper piston area
- pad friction coefficient
- effective rotor radius
- caliper mount geometry
- adapter material/thickness
- bolt sizes/grades and thread engagement
- knuckle/caliper interfaces

Verify more than peak von Mises stress. Evaluate as applicable:

- bearing stress
- net section / tear-out
- edge distance
- bracket deflection
- bolt shear/tension interaction
- thread engagement
- contact/slip assumptions
- stress concentration and fatigue sensitivity

Document model idealization, boundary conditions, load derivation, mesh/convergence choices, material properties, and acceptance criteria. A colorful FEA plot is not verification by itself.

## BBK hydraulic discipline

Separate brake-torque distribution from pedal feel/master-cylinder sizing.

Front/rear brake torque depends on caliper piston area, pad friction assumptions, effective rotor radii, and hydraulic pressure distribution. Master-cylinder bore mainly changes hydraulic leverage, pedal force, and pedal travel across the system; it does not by itself correct front/rear balance.

Before permanent release, recover/recompute both.

## BBK manufacturing/release rule

Do not release the permanent front adapter until structural verification and system-level hydraulic implications are documented.

Do not restart the rear design from a blank sheet. First compare any cleaner alternative against the known-valid LS430/Nissan architecture and require a meaningful improvement in serviceability, manufacturability, or irreversible modification.

# Electric Power Steering

## Authoritative checkpoint

Available hardware includes multiple Nissan Versa column-EPS components, an MR2 Spyder electro-hydraulic pump/reservoir assembly, spare Celica steering hardware, and at least one spare Celica column.

The EPS project did **not** stall because electric assist was considered infeasible. It stalled because there was no clean, drawing-controlled mechanical integration plan between donor EPS shaft/spline geometry and the Celica steering column/intermediate shaft.

The user does not want a one-off `cut here / weld there` solution merely because it can be made to turn.

## EPS governing rule

> Do not cut a spare Celica column until the complete mechanical load path, shaft-interface strategy, mounting scheme, and packaging are defined.

The preferred result should look like a designed, inspectable, reproducible assembly.

## EPS DFM priorities

Prefer where practical:

- retaining OEM spline interfaces
- commercially available spline couplers or steering U-joints
- useful OEM donor/intermediate-shaft segments
- bolted/clamped/keyed interfaces
- machined adapters that can be dimensioned, toleranced, inspected, and reproduced
- reversible prototype work on spare components
- full CAD assembly before destructive modification

Avoid as the default architecture:

- arbitrary shaft cuts
- hand-ground D-shafts
- undocumented sleeve/weld splices
- geometry dependent on one fabricator's fit-up
- modification of the installed/original column before the spare architecture is proven

A controlled welded component is not categorically forbidden if later engineering shows it is the best solution, but `cut and weld until it fits` is not an acceptable starting design.

## EPS architecture candidates

### Column-mounted EPS

Nissan Versa hardware is the active R&D candidate. Solve:

- Celica and donor spline/interface identification
- axial packaging
- motor clearance
- steering-wheel position
- shaft/U-joint geometry
- EPS housing reaction-torque mount
- dash/column structure interface
- telescoping/collapse behavior
- torque capacity and fatigue
- electrical standalone/control behavior after mechanical architecture closes

### MR2 Spyder electro-hydraulic fallback

Retain the Celica hydraulic rack and replace the engine-driven pump with the electric MR2 pump/reservoir assembly.

Treat this as the lower-risk fallback/reference architecture. It avoids column spline adaptation but retains hydraulic lines/fluid and requires pump packaging, electrical power, line adaptation, and control work.

## EPS decision gate

The meaningful decision is:

> Can column EPS be integrated with controlled, reproducible mechanical interfaces that meet the project's DFM and safety standard without unacceptable Celica-column modification?

If yes, continue column EPS.

If no, the MR2 electro-hydraulic architecture is a valid fallback rather than forcing an ugly column solution.

## EPS verification before vehicle installation

Verify at minimum:

- shaft/coupler torque capacity and margin
- clamp/fastener anti-slip capacity
- alignment/runout and U-joint angles
- housing reaction mount loads/stiffness
- full lock-to-lock rotation without binding
- affected telescoping/collapse behavior
- manual steering with assist disabled
- electrical protection/fault behavior
- assist behavior across operating conditions before normal road use

Steering is safety-critical. Prototype convenience does not override inspectability or robustness.

# P4 e-AWD / Hybrid Rear Axle

## Authoritative checkpoint

This project is **parked**, but the architecture is deliberately preserved in `eawd/PROJECT.md` because enough engineering work has been done that losing the rationale would create substantial rework later.

Current tentative architecture:

- Toyota/Lexus Q211 MGR rear transaxle;
- custom first-stage regear targeting roughly 4.07:1 overall ratio rather than the stock 6.859:1;
- 2010–2015 Gen-3 Prius inverter/PCU with standalone controller;
- Ford C-Max Hybrid, non-Energi, compact ~281 V high-power battery;
- Ford BECM/contactors retained if standalone CAN control is practical;
- custom/modified rear cradle using Matrix AWD geometry as a future hard-point reference;
- single rear motor + open differential;
- preserve the OEM fuel tank if practical;
- off-car development first, vehicle integration last.

## e-AWD governing rule

> Do not let enthusiasm for cheap donor hardware outrun the packaging gate.

The only justified speculative acquisition while the project is parked is a cheap, complete Q211 with its output stubs, brackets, pigtails, HV cable pieces and useful inner-CV hardware.

After acquisition:

- weigh it;
- photograph it;
- 3D scan it;
- manually measure critical datums;
- store it.

Do **not** buy the battery, commission gears, create a cradle, or create dashboard work merely because the Q211 has been acquired.

## e-AWD evidence discipline

Treat as **TENTATIVE** until measured/proven:

- Q211 10,000 rpm mechanical design limit;
- exact custom helical gear geometry;
- actual 281 V direct-bus torque/power envelope;
- Matrix/Q211 CV spline interchange;
- final battery dimensions/packaging;
- C-Max BECM minimum standalone message set;
- final cradle architecture and fuel-tank clearance;
- final system mass and cost.

Treat the ~31T/32T first-stage pair as a ratio/center-distance concept only. A gear designer must close module, helix, pressure angle, profile shift, face width, hub/spline geometry, bearing load, contact, material, heat treatment, lubrication and manufacturability before release.

## e-AWD battery / CAN discipline

The desired Ford strategy is to preserve the OEM BECM's cell monitoring, temperature monitoring, current sensing, power limits, cooling control, precharge and contactor supervision.

Reverse-engineer only the message contract needed to operate it standalone. Prefer an intact, electrically healthy but mechanically failed C-Max Hybrid as a future CAN/reference donor if one can be bought cheaply.

Do not bypass BECM limits just to obtain rear torque.

## e-AWD high-voltage discipline

Traction-battery and inverter work involves lethal voltage and stored energy. Preserve or intentionally replace OEM safety functions with equivalent engineered protection, including as applicable:

- service disconnect;
- precharge;
- main contactors;
- correctly rated fusing;
- HV interlock strategy;
- insulation/creepage/clearance;
- guarded terminals and connectors;
- de-energization verification;
- fault shutdown;
- battery/inverter/motor temperature limits;
- physical protection of HV cable routes.

Do not conduct high-power unloaded motor testing on an improvised fixture.

## e-AWD promotion gate

Do not add `SIDE-EAWD-*` tasks until the user explicitly revives the project. The first meaningful gate after Q211 acquisition is:

> Can the Q211 be integrated into the Celica rear geometry, using the Matrix assembly as a suspension/hard-point reference, while preserving acceptable fuel-tank/floor/exhaust packaging and axle geometry?

If no, keep the project parked or close it without escalating sunk cost.

If yes, only then does inverter bench work deserve active project status.

## Cross-project boundaries

- Baseline owns restoration of conventional hydraulic PS now.
- EPS is optional R&D and does not block Baseline.
- BBK and EPS do not redefine Street Build completion unless deliberately adopted later.
- P4 e-AWD is parked and does not redefine Street Build completion, rear-suspension architecture, fuel-system architecture, or current harness work unless explicitly adopted later.
- A future custom rear cradle may interact with suspension, exhaust, fuel-tank, battery and wiring work; record explicit interfaces rather than silently changing other repositories.

Create explicit cross-project dependencies rather than duplicate tasks.

## Definition of done

Before marking an active BBK or EPS task done, or future e-AWD task if that project is promoted:

1. preserve the useful result in the relevant `PROJECT.md` or supporting engineering record;
2. record exact parts/geometry/analysis assumptions where relevant;
3. update decisions/current architecture;
4. create only genuinely actionable follow-ons;
5. reconcile blocked/ready tasks;
6. preserve CAD/FEA/test/source references needed to reproduce the work.

For parked-project research, update the durable Markdown checkpoint without creating attention/task debt unless the user explicitly wants the project active.

## End-of-session reconciliation

After meaningful work ask:

- What exact interface or fact did we establish?
- What remains assumed?
- Did a design become selected, rejected, or merely more plausible?
- What verification is still required before fabrication/installation?
- What task state changed?
- Is the record sufficient to resume without reconstructing the problem from memory?

Do not preserve chat transcripts as project documentation. Convert the useful outcome into concise engineering state.

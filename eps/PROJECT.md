# Celica EPS — Resume Here

## Purpose

Develop an electric-assist steering conversion for the 2000 Toyota Celica GT-S that reduces dependence on the engine-driven hydraulic system **without turning the steering column into a one-off cut-and-weld fabrication exercise**.

The project is feasible in principle. The unresolved problem is mechanical integration and design-for-manufacture.

## Authoritative Current Checkpoint

### Available hardware

Known hardware available for investigation includes:

- multiple Nissan Versa electric-power-steering components/units;
- an MR2 Spyder electro-hydraulic power-steering pump/reservoir assembly;
- spare Celica hydraulic power-steering racks/pumps;
- at least one spare Celica steering column.

Exact donor years, part numbers, shaft/spline dimensions, and current condition still need to be inventoried before design release.

Ownership of these parts does **not** select an architecture by itself.

## Where the Project Actually Stopped

The project stalled at the mechanical interface between an OEM EPS assist unit and the Celica steering column/intermediate shaft.

The main blockers were:

- no clean concept for adapting the donor EPS shaft geometry/splines to the Celica column;
- no spline-cutting/broaching capability available at the time;
- reluctance to cut up spare Celica steering columns before a complete interface plan existed;
- common retrofit examples tended toward one-off `cut here / weld there` fabrication rather than a repeatable engineered interface.

The feasibility of column-mounted EPS itself was not the concern. The concern was whether it could be integrated cleanly, safely, serviceably, and in a way that could be documented/reproduced.

## Governing Design Philosophy

> Do not cut a spare Celica column until the complete mechanical load path and interface strategy are defined.

The preferred solution should look like a designed assembly rather than a hot-rod steering-column splice.

### DFM requirements

Prefer:

- OEM splined interfaces retained wherever practical;
- commercially available spline couplers/U-joints where a standard spline exists;
- bolted, clamped, keyed, or otherwise inspectable mechanical joints;
- machined adapters that can be dimensioned, toleranced, inspected, and reproduced;
- reversible modification of donor/spare components during prototype development;
- use of the spare Celica column as a bench integration fixture before touching the installed column;
- preservation of OEM collapse/telescoping/safety behavior to the extent practical;
- explicit verification of torque capacity, clamp/fastener integrity, alignment, and fatigue-critical interfaces.

Avoid where practical:

- arbitrary shaft cuts without a drawing-controlled replacement interface;
- welded shaft extensions/splices as the default design solution;
- hand-ground D-shafts or geometry that depends on an individual fabricator's fit-up;
- modifying the installed/original column before the prototype architecture is proven.

A welded component is not automatically forbidden if a later engineering review demonstrates it is the best controlled solution, but `cut and weld until it fits` is not an acceptable starting architecture.

## Architecture Candidates

### Candidate A — column-mounted electric assist

Use an OEM column-EPS mechanism, currently with Nissan Versa hardware available for investigation, integrated into the Celica steering-column load path.

Potential advantages:

- eliminates the engine-driven hydraulic pump and associated belt load/plumbing;
- assist is generated inside the cabin/column system rather than at the rack;
- potentially clean long-term architecture if the shaft and mounting interfaces can be solved properly.

Primary engineering work:

- donor EPS input/output shaft identification;
- Celica column/intermediate-shaft spline identification;
- axial packaging and steering-wheel position;
- structural mounting of EPS housing/reaction torque into Celica dash structure;
- collapse/telescoping behavior;
- shaft adapter/coupler architecture;
- electrical standalone behavior and assist-control strategy.

This is the **R&D architecture** that originally stalled on mechanical integration.

### Candidate B — MR2 Spyder electro-hydraulic assist

Retain the Celica hydraulic rack but replace the engine-driven pump with the available MR2 Spyder electric hydraulic pump/reservoir assembly.

Potential advantages:

- avoids steering-column spline adaptation;
- preserves the existing Celica rack/steering-column mechanics;
- separates steering assist from the engine accessory drive;
- likely lower mechanical integration risk.

Tradeoffs:

- retains hydraulic lines, fluid, rack seals, and hydraulic-system maintenance;
- pump packaging, power supply/current demand, line adaptation, and control still need engineering;
- does not achieve the same mechanical simplification as true column EPS.

Treat this as the **lower-risk fallback/reference architecture**, not automatically the selected solution.

## External Precedent / What It Tells Us

Toyota Prius EPS documentation confirms the typical column-EPS architecture: steering torque is sensed at the column, an electric motor/reduction mechanism provides assist torque, and assist is modulated by the EPS controller. General Prius retrofit documentation also shows that the difficult vehicle-specific part is usually adapting the OEM EPS shaft/column to the recipient vehicle's steering shaft.

A surviving forum post from a 2002 seventh-generation Celica owner also documents investigation of a Corolla EPS-column swap into the Celica. The exact Prius-in-seventh-gen-Celica installation remembered from earlier research has **not yet been re-located**, so treat that specific precedent as anecdotal until recovered.

The important engineering conclusion remains valid regardless: OEM column EPS can be transplanted; the Celica project needs a clean mechanical-interface architecture rather than proof that electric assist can function.

## Resume Here — Next Useful Actions

1. **Inventory the actual EPS hardware.** Record Versa donor part numbers/year markings, input/output shaft geometry, ECU/module configuration, and MR2 pump identification.
2. **Identify the Celica steering interfaces.** Measure/research the spare column and intermediate shaft: spline count/major diameter, flats, pinch-bolt features, telescoping/collapse interfaces, mounting datums, and available straight shaft lengths.
3. **Identify donor EPS interfaces.** Determine whether the Versa input/output splines correspond to commercially available couplers or other OEM intermediate-shaft parts.
4. **Build a mechanical interface matrix.** Compare direct spline couplers, OEM donor intermediate-shaft segments, clamp-style U-joints, machined adapters, and any unavoidable welded option against DFM/safety/serviceability criteria.
5. **CAD the complete column assembly before cutting anything.** Use the spare Celica column and donor EPS geometry to establish motor clearance, steering-wheel position, mount reaction path, and collapsibility.
6. **Bench-prototype the selected interface.** Only after the CAD architecture closes should a spare column or donor shaft be modified.
7. **Separately characterize the electrical/control problem.** Do not allow controller/CAN work to distract from solving the mechanical architecture first.

## Immediate Decision Gate

The next meaningful decision is not simply `Versa EPS or MR2 pump?`

It is:

> Can a column-EPS architecture be produced using controlled, reproducible mechanical interfaces that satisfy the project's DFM standard without unacceptable modification of the Celica column?

If yes, continue the column-EPS design.

If no, the MR2 electro-hydraulic architecture remains a practical fallback that captures much of the desired accessory-drive simplification without forcing an ugly steering-column solution.

## Verification Before Vehicle Installation

Before any column-EPS assembly is installed in the car, verify at minimum:

- steering-shaft torque capacity and safety factor;
- fastener/coupler clamp capacity and anti-slip features;
- alignment/runout and U-joint operating angles;
- mount reaction loads and local body/bracket stiffness;
- full steering rotation without binding;
- telescoping/collapse behavior affected by the modification;
- fail-safe manual-steering behavior with assist disabled;
- electrical power protection and fault behavior;
- assist behavior across low/high vehicle speeds before normal road use.

Steering is safety-critical; prototype convenience does not override inspectability or mechanical robustness.

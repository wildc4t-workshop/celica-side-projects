# Celica P4 e-AWD / Hybrid Rear Axle — Resume Here

## Project State

**Status:** parked / documented concept  
**Dashboard status:** one passive donor-watch task only (`SIDE-EAWD-001`); no execution work  
**Target vehicle:** 2000 Toyota Celica GT-S, turbo 2ZZ + E153 manual FWD drivetrain  
**Current rear-drive baseline:** Mitsubishi Y61 rear unit + matched OEM Mitsubishi rear inverter  
**Current battery baseline:** Ford C-Max Hybrid, non-Energi, ~281 V / 1.4 kWh high-power HEV pack  
**Current high-speed strategy:** retain stock 7.065 gearing and develop a staged one-side inboard halfshaft disconnect  
**Temporary speed rule before disconnect:** approximately 90 mph vehicle limit  
**Rough DIY budget:** prefer about **$6,000**, with a planning range of roughly **$5,000–$7,000** until real donor/package/fabrication costs are known  
**Mass gate:** prefer **<= ~275 lb net addition**; investigate up to ~300 lb before rejecting the Y61 architecture  

This project remains intentionally parked. It is not part of Baseline or the committed Street Build and should not compete with current work. The purpose of this file is durable engineering memory, not authorization to start buying or fabricating.

Detailed current architecture: [`CURRENT_ARCHITECTURE.md`](CURRENT_ARCHITECTURE.md)  
Trade calculations and rejected alternatives: [`TRADE_STUDY_2026-09-08.md`](TRADE_STUDY_2026-09-08.md)  
Current evidence record: [`SOURCES_CURRENT.md`](SOURCES_CURRENT.md)  
Legacy Q211 / Prius-inverter source record: [`SOURCES.md`](SOURCES.md)

The prior Q211 + Gen-3 Prius-inverter + custom ~4.07 regear architecture is **superseded**. Its historical state remains recoverable from Git history and its source record is retained because Q211/OpenInverter research may still be useful later.

## Purpose

Add a compact P4 electric rear axle while retaining:

- turbo 2ZZ + E153 driving the front wheels;
- OEM gasoline fuel tank if packaging allows;
- normal front-drive operation if the rear system is unavailable;
- a lightweight street-car rather than EV-conversion operating model.

The rear system is a **traction allocator / torque buffer**, not an EV-range system.

Desired effects:

- AWD launch traction;
- off-boost torque fill;
- rear torque while the E153 clutch is open during shifts;
- reduced front-tire longitudinal burden during corner exit;
- improved rain / low-mu traction;
- modest midrange assistance;
- mild lift/brake regen;
- optional stronger driver-requested regen;
- optional through-the-road charging during cruise.

The target sensation remains:

> It still feels like a turbo FWD Celica, except it launches cleanly, responds immediately, and does not completely lose propulsion during shifts.

## Current Architecture

```text
                         FRONT

 turbo 2ZZ  ->  E153 LSD  ->  front wheels
       |
       | engine torque / rpm / throttle / clutch / faults
       v
     CELICA VCU
       |             ^
       | torque req  | SOC / power limits / battery faults
       v             |
 Mitsubishi OEM rear inverter  <----  Ford C-Max Hybrid BECM / battery
       |
       | three phase
       v
 Mitsubishi Y61 motor / 7.065 reduction / open differential
       |
       +--> future one-side inboard synchronized disconnect
       |
 rear CVs / custom axle bars as required
       |
 Matrix/Celica-compatible hubs / suspension geometry

                         REAR
```

## Why Y61 is the Current Baseline

The system was sized from rear tire force and useful speed range rather than from maximum donor kW.

Using a 235/35R18 tire, 95% assumed reduction/differential efficiency, 195 Nm Y61 torque and 7.065 reduction:

```text
axle torque ~= 195 * 7.065 * 0.95 ~= 1,309 Nm
rear tire thrust ~= 947 lbf
```

A 70 kW motor can hold approximately that full thrust through the mid-30-mph region before becoming power-limited.

That is a strong match for the Celica mission:

- roughly the launch thrust of the much lighter Q610;
- substantially more 25–60 mph rear-force persistence than Q610;
- no need for a 100–250 kW EV drive unit;
- mature conversion precedent for commanding the OEM Mitsubishi inverter over CAN.

The intentional trade is mass. Community teardown information puts the Y61 motor/differential/bracket assembly around ~133 lb versus ~91 lb for Q610. The project accepts that penalty provisionally because the matched OEM inverter means Mitsubishi retains resolver/current/field-weakening/motor-protection responsibility.

The custom engineering effort should answer:

> What rear torque should the Celica request?

not:

> How do we make an unfamiliar PMSM produce controlled torque at all?

## Alternative Determinations

### Toyota Q610 / 4NM — HOLD / watch list

Approximate comparison on the Celica tire:

- ~40 kW;
- ~120 Nm;
- 10.781:1 stock reduction;
- ~91 lb module;
- ~889 lbf rear thrust;
- full-thrust/power crossover ~21.5 mph;
- 10,000 motor rpm at only ~67.5 mph.

Q610 remains the lightweight technical ideal if cheap, documented standalone torque/regen control becomes turnkey or if Y61 packaging/mass fails. It is not the current development path because a Prius/OpenInverter approach would make resolver calibration, motor characterization, FOC tuning, field weakening and voltage-envelope validation part of this project.

### Toyota Q211 / 2FM — SUPERSEDED final-drive direction

Approximate comparison:

- ~50 kW;
- ~130 Nm;
- 6.859:1 stock ratio;
- ~92 lb;
- ~613 lbf rear thrust with stock gearing.

The former ~4.072 custom-regear concept reduced calculated rear thrust to only ~364 lbf while adding custom helical-gear cost, lubrication and validation work. It solved overspeed by sacrificing too much of the traction benefit that P4 is supposed to provide.

## Battery — Ford C-Max Hybrid

The C-Max **Hybrid, non-Energi** pack remains the selected reference because it is a high-power buffer rather than an EV-energy pack.

DOE/INL reference data:

- 76 series cells;
- 281.2 V nominal;
- 5.0 Ah;
- 1.4 kWh;
- 76 lb complete-pack benchmark;
- active forced-air cooling.

Two DOE-tested nominally similar 2013 packs demonstrate why the actual donor/BECM must remain authoritative:

| DOE test vehicle | 10 s discharge @ 50% DOD | 10 s charge @ 50% DOD |
|---|---:|---:|
| VIN 5138 | 66.0 kW | 47.8 kW |
| VIN 8698 | 56.2 kW | 42.2 kW |

Do **not** enlarge the battery merely to justify a 70 kW Y61. The VCU should clip rear torque to the Ford BECM's instantaneous discharge/charge limits.

### Preferred battery-control philosophy

Preserve Ford's intelligence if practical:

- cell-voltage monitoring;
- battery temperatures;
- pack current;
- discharge/charge limits;
- contactor/precharge supervision;
- cooling control;
- fault logic.

The Celica VCU should learn the minimum CAN message contract needed to operate the BECM standalone rather than becoming a replacement BMS.

### Battery CAN reverse-engineering workflow

If a complete electrically healthy but mechanically failed C-Max Hybrid is available cheaply, it remains the preferred reference donor.

Capture two views of the same events:

```text
C-Max HS-CAN
   |
   +--> raw CAN logger / SavvyCAN
   |
   +--> FORScan named battery PIDs
```

Capture at minimum:

1. asleep/off;
2. unlock/bus wake;
3. ignition on but not READY;
4. transition into READY;
5. READY with low electrical load;
6. acceleration/discharge;
7. lift-off regen;
8. brake regen;
9. steady cruise;
10. fan/thermal changes if practical;
11. READY-to-off shutdown;
12. several minutes after shutdown.

Bench progression:

1. service disconnect removed; low-voltage power/CAN/interlocks only;
2. observe autonomous BECM broadcasts/faults;
3. replay captured vehicle traffic;
4. remove message groups until the minimum required set is isolated;
5. identify state/counters/checksums for required IDs;
6. replace replay with the VCU state machine;
7. only then perform controlled HV precharge/contactor testing using the OEM safety architecture.

## OEM Mitsubishi Rear Inverter

The matched OEM rear inverter is a major reason Y61 is selected.

OpenInverter community documentation already identifies:

- required heartbeat traffic;
- rear torque command;
- torque report;
- motor RPM report;
- HV report;
- motor temperatures;
- phase-current reporting;
- forward/reverse direction handling.

The Celica VCU therefore commands primarily:

```text
requested rear motor torque
```

rather than phase current or throttle percentage.

Feedback should include as available:

- motor RPM;
- actual torque/current;
- DC voltage/current;
- motor temperature;
- inverter temperature;
- inverter ready/fault state.

## Stock Gearing / Overspeed / Temporary Vehicle Limit

Stock 7.065 gearing is deliberately retained because aggressive low-speed multiplication is a feature.

Calculated Y61 motor speed on 235/35R18:

| Vehicle speed | Y61 motor speed |
|---:|---:|
| 60 mph | ~5,820 rpm |
| 80 mph | ~7,760 rpm |
| 90 mph | ~8,730 rpm |
| 95 mph | ~9,220 rpm |
| 100 mph | ~9,700 rpm |

Community material places the rear motor near the ~10,000-rpm region. That is **TENTATIVE** until better evidence or controlled donor validation exists.

Current rule:

> **Before a validated disconnect, software-limit the car to approximately 90 mph.**

This permits street development, launches and autocross but not unrestricted track-day use.

## Preferred Disconnect

Current concept: external, one-side, inboard halfshaft disconnect using the open differential.

Desired characteristics:

- bearing-supported inboard shaft;
- splined sliding dog clutch;
- serviceable independently of the Y61;
- positive engaged/disengaged sensing;
- electrical actuation eventually;
- rear torque forced to zero whenever state is unresolved.

The attraction is that the disconnect sees axle-side high torque but comparatively low rotational speed rather than being located inside the high-rpm motor/reduction unit.

### Reconnection concept

1. rear torque = zero;
2. calculate target differential/motor speed from wheel speeds;
3. spin Y61 to synchronize the disconnect sides;
4. confirm small delta-RPM;
5. engage dog clutch;
6. confirm engagement;
7. ramp rear torque back in.

### Disconnection concept

1. ramp rear torque to zero;
2. verify near-zero transmitted torque;
3. release dog clutch;
4. allow motor/carrier to coast down.

### Baby-step disconnect development

- Rev 0: manual/stationary disconnect;
- verify open-diff kinematics, internal relative speed and lubrication;
- Rev 1: electrically actuated stationary engagement;
- Rev 2: low-speed synchronized engagement;
- Rev 3: progressively expand dynamic disconnect/reconnect speed;
- remove the temporary speed limiter only after verified operation and fault handling.

Important open item: protecting the rotor does not automatically prove the differential side gears/pinions are happy at the internal relative speed created by one-side disconnection. Calculate and validate that separately.

## Vehicle Dynamics / Controls

The first POC should be simple. Do not start by reproducing a complete OEM AWD/stability system.

### Straight-line modes

- zero-torque standby;
- launch assist;
- front-slip reduction / rear torque addition;
- off-boost fill;
- clutch-open shift fill;
- mild lift regen;
- brake-coordinated regen;
- push-button stronger regen/charge request;
- through-the-road cruise charging;
- thermal/SOC/power derating;
- immediate fault inhibit.

### Cornering principle

P4 helps even without visible front wheelspin because the front tires share lateral and longitudinal grip:

```text
Fx^2 + Fy^2 <= (mu*N)^2
```

Moving propulsion rearward leaves more front friction capacity available for steering.

Initial corner logic should be conservative:

- entry: little/no positive rear torque;
- mid-corner: limited rear torque based on available grip;
- exit: increase rear torque as steering unwinds;
- oversteer/yaw tendency: rapidly reduce positive rear torque;
- do not begin with corrective negative rear torque.

Later useful inputs:

- four wheel speeds;
- steering angle;
- yaw rate;
- lateral acceleration;
- brake pressure/state.

The Y61/open-differential architecture is front/rear traction allocation, **not** true left/right torque vectoring.

### Rain / low-mu behavior

As road mu falls, less rear force is required to use available traction. A ~950-lbf Y61 axle therefore becomes proportionally more capable in rain/snow than on a dry high-grip surface.

Priorities:

- gentle torque slew;
- wheel-slip limits;
- stability before maximum acceleration;
- battery/inverter/thermal limits remain authoritative;
- no direct driver-to-inverter torque path.

## Performance Interpretation

The first-pass acceleration model is for architecture ranking, not time-slip prediction.

Under optimistic assumptions (perfect traction control, synthetic ~500-whp front curve, simplified mass/drag/shift behavior), a Y61-class ~70 kW / ~950 lbf rear system fell into theoretical:

- sub-4-second 0–60 territory;
- mid/high-11-second quarter-mile territory.

Do not treat those as promises. The durable conclusion is that this motor class appears capable of crossing the project's desired serious-street-car benchmark **without** a 100+ kW rear system.

## Packaging / Mass / Cost Gates

Provisional core mass:

- Y61 motor/diff/brackets: ~133 lb community benchmark;
- OEM rear inverter: ~20 lb benchmark;
- C-Max battery: 76 lb;
- core three components: ~229 lb before mounting, cooling, axles, disconnect, HV/LV cabling and cradle delta.

Therefore complete installed mass currently looks roughly **270–300 lb** until CAD and weighing prove otherwise.

Current gates:

```text
net mass addition: prefer <= ~275 lb; investigate to ~300 lb before rejection
finished DIY cost: prefer <= ~$6,000; rough planning band ~$5,000–$7,000
retain OEM fuel tank if practical
development off-car; integration last
```

Reconsider Y61 if:

- output-center/package geometry cannot coexist with acceptable rear suspension/tank/floor geometry;
- installed mass cannot reasonably stay near/under ~300 lb;
- OEM inverter control is materially less mature than current evidence indicates;
- C-Max BECM/battery becomes disproportionate work or a better lightweight HEV pack exists when revived;
- the one-side disconnect produces unacceptable differential speed/lubrication/durability;
- cost grows materially outside the grassroots band.

If Y61 fails mainly on mass or packaging, Q610 is the first architecture to revisit.

## Development Sequence When Revived

### Phase 0 — donor/package characterization

- acquire Y61 only if the project is explicitly revived or an unusually good complete donor package appears;
- keep motor/diff, matched rear inverter, brackets, HV cable, pigtails and useful inner-CV hardware together;
- weigh, photograph, 3D scan and hand-measure critical datums;
- verify exact motor/inverter part numbers and donor year;
- measure Matrix/Celica rear geometry before buying the rest of the system.

### Phase 1 — mechanical feasibility

- establish common coordinate system for Celica/Matrix/Y61;
- preserve suspension hard points;
- place Y61 by output/CV geometry;
- sweep axle motion;
- verify fuel-tank/floor/exhaust clearance;
- estimate real cradle/mount mass;
- kill/rethink here if the body/package compromise is unacceptable.

### Phase 2 — OEM inverter low-energy CAN POC

- power the inverter controls safely;
- reproduce required heartbeat/status exchange;
- command tiny forward/reverse torque on a proper fixture;
- prove zero-torque state, speed/temperature feedback and fault inhibit;
- do not begin with high-power unloaded testing.

### Phase 3 — C-Max battery/BECM CAN

- use intact reference donor/logging workflow if available;
- isolate minimum standalone message contract;
- prove safe wake/READY/shutdown, precharge/contactors/interlocks;
- verify real discharge/charge power limits from the actual pack.

### Phase 4 — integrated HV bench POC

- battery -> fuse/contactors/precharge -> Mitsubishi inverter -> Y61;
- tightly limit torque/current/speed initially;
- prove motoring and regen;
- map pack/inverter/motor thermal and power limits.

### Phase 5 — disconnect POC

- manual/stationary first;
- verify open-diff internal speeds/lubrication;
- add actuator and sensing;
- low-speed synchronization;
- progressively expand speed.

### Phase 6 — vehicle integration

Only after the above closes:

- finish cradle/axles;
- mount battery/inverter;
- install final HV/LV/cooling;
- integrate VCU with EMU Black and chassis signals;
- begin with conservative rear torque and ~90 mph vehicle cap;
- expand launch, shift-fill, regen, cornering and weather functions one at a time through logged validation.

## Current Decisions

- **DEC-SIDE-EAWD-001 — SELECTED:** P4 electric rear axle; no mechanical front/rear driveline coupling.
- **DEC-SIDE-EAWD-002 — SELECTED:** Mitsubishi Y61 + matched OEM rear inverter is the current drive-unit baseline.
- **DEC-SIDE-EAWD-003 — SELECTED:** Ford C-Max Hybrid non-Energi remains the battery reference.
- **DEC-SIDE-EAWD-004 — SELECTED:** retain stock Y61 gearing and solve high-speed operation with a staged disconnect.
- **DEC-SIDE-EAWD-005 — HOLD:** Toyota Q610 is the lightweight future optimization if control becomes turnkey or Y61 fails package/mass gates.
- **DEC-SIDE-EAWD-006 — SUPERSEDED:** Toyota Q211 + custom ~4.07 regear is no longer the preferred final architecture.

## Resume Here — Near-Term Rule

While parked, `SIDE-EAWD-001` is only a passive donor watch.

Watch for:

- later Outlander PHEV Y61 rear drive units;
- matched Mitsubishi rear traction inverter;
- factory motor/inverter HV cable, pigtails, brackets and useful rear inner-CV hardware;
- mechanically failed but electrically healthy C-Max Hybrid non-Energi reference cars/packs.

Do not buy a complete system merely because the concept now looks promising.

When explicitly revived, the first real gate is:

> **Can the Y61 be packaged against the Matrix/Celica rear hard points while preserving acceptable axle geometry, fuel-tank/floor/exhaust space, and a credible <=~300-lb total-system mass?**

If yes, the next gate is low-energy OEM-inverter CAN control on the bench.

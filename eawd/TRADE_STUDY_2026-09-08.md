# Celica P4 e-AWD — Rear Drive Architecture Trade Study

**Checkpoint:** 2026-09-08  
**Status:** durable parked-project engineering record  
**Decision level:** selected current direction, subject to packaging/mass verification  

## 1. Question being answered

The design objective is not to maximize electric power. It is to make the fastest practical street Celica while adding the minimum electric-system mass and avoiding unnecessary motor-control R&D.

The turbo 2ZZ + E153 remains the primary drivetrain. The rear electric axle exists to consume traction capacity that the front axle cannot use efficiently, especially during:

- launch;
- low-speed boost onset;
- manual-transmission shifts;
- corner exit, where the front tires are already carrying lateral load;
- rain / low-mu conditions;
- lift/brake regen and optional through-the-road charging.

The desired car still feels like a turbo FWD Celica. P4 should remove the worst FWD traction penalties without turning the car into a heavy EV conversion.

## 2. Governing optimization

The useful rear system is defined by two independent requirements:

1. **Peak rear tire thrust** — how much of the otherwise-unused rear traction can be consumed at low speed.
2. **Rear mechanical power** — how long that thrust can be sustained as road speed rises.

For a given rear tire force:

```text
P = F * v
```

Therefore large launch force does not require large power at very low vehicle speed. Power primarily determines the speed at which the rear axle transitions from torque-limited to power-limited.

The project should therefore optimize:

```text
rear tire thrust per installed pound
+ enough kW to carry that thrust through the useful low/mid-speed region
+ low controls/integration burden
```

rather than maximum donor-motor nameplate power.

## 3. Common calculation assumptions

These are calculation inputs, not released vehicle specifications.

- tire: 235/35R18;
- calculated unloaded tire diameter: ~621.7 mm;
- calculated radius: ~0.31085 m;
- mechanical reduction/differential efficiency used for first-pass comparisons: 95%;
- dry-road comparison coefficient: mu ~= 1.0 unless otherwise stated;
- front engine: built turbo 2ZZ, roughly 500 whp design envelope, 8,400 rpm limit;
- front torque control: assume EMU/DBW/boost-by-gear/wheel-speed control can keep the front tires near their usable traction limit rather than allowing uncontrolled wheelspin.

Synthetic turbo curves were used only to test whether the likely final turbo choice materially changes the P4 sizing problem. A fast GT28-frame / GTX28-class curve and a slower 20G / FP-Blue-class curve both produced far more theoretical front tractive force at low road speed than the front tires could accept. Therefore the rear-system sizing is primarily a tire-allocation problem rather than a turbo-spool problem once the car is under load.

## 4. Candidate comparison

### 4.1 Mitsubishi Y61 — selected current direction

Reference hardware: later Outlander PHEV rear drivetrain.

Published/community baseline:

- rear motor model: Y61;
- later rating: ~70 kW;
- peak torque: ~195 Nm;
- rear reduction: 7.065:1;
- open differential;
- dedicated OEM rear inverter available;
- OEM rear inverter nominal architecture: ~300 V class;
- standalone OEM-inverter CAN control has substantial OpenInverter/ZombieVerter community precedent.

First-pass Celica tire calculation:

```text
axle torque ~= 195 Nm * 7.065 * 0.95
            ~= 1,309 Nm

rear tire thrust ~= 1,309 Nm / 0.31085 m
                 ~= 4,210 N
                 ~= 947 lbf
```

At 70 kW shaft power and 95% mechanical efficiency, nominal wheel power is roughly 66.5 kW. Full ~947 lbf thrust can therefore be sustained to approximately:

```text
v ~= P/F ~= 35 mph
```

This is unusually well matched to the project mission: very strong launch assistance without requiring a 100+ kW rear system, while retaining useful rear propulsion through the 25–60 mph region.

### 4.2 Toyota Q610 / 4NM — lightweight technical alternative, not current baseline

Reference values:

- ~40 kW;
- ~120 Nm;
- 10.781:1 stock reduction;
- ~41.1 kg / ~91 lb complete MGR benchmark.

First-pass Celica tire calculation:

```text
rear tire thrust ~= 889 lbf
constant-torque/power crossover ~= 21.5 mph
```

Benefits:

- nearly Y61 launch thrust at approximately 42 lb less eAxle mass;
- excellent thrust-per-eAxle-pound;
- 40 kW is enough to capture most of the 0–30 benefit available from its own ~890 lbf torque ceiling.

Why it is not selected today:

- stock 10.781 gearing creates severe motor-speed exposure; 10,000 motor rpm corresponds to only ~67.5 mph on 235/35R18;
- Toyota rear-motor inverter control is not a simple dedicated standalone CAN subsystem like the Mitsubishi architecture;
- a Prius/OpenInverter solution is plausible, and Toyota rear-MGR precedent exists, but Q610-specific resolver/FOC/field-weakening/voltage calibration would become a substantial motor-control R&D project;
- the project already contains enough custom work in chassis, VCU, battery, disconnect and safety integration.

Current determination:

> Do not volunteer to become the Q610 motor-control development program merely to save ~40 lb. Keep Q610 on the watch list and reconsider immediately if the community demonstrates a documented inexpensive standalone torque/regen solution.

### 4.3 Toyota Q211 / 2FM — superseded final-drive direction

Reference values:

- ~50 kW;
- ~130 Nm;
- 6.859:1 stock reduction;
- ~41.8 kg / ~92 lb module.

Stock Celica-tire calculation:

```text
rear tire thrust ~= 613 lbf
10,000 motor rpm ~= 106 mph
```

The prior project concept used a custom ~4.072:1 first-stage regear to make permanent mechanical connection compatible with very high Celica road speed. That regear reduced low-speed rear thrust to only about:

```text
~364 lbf
```

while creating custom-helical-gear cost, lubrication and validation work.

Current determination:

> Q211 remains useful as a cheap reference/experimental MGR but is no longer the preferred final architecture. Regearing solved overspeed by sacrificing too much of the launch-traction benefit that the P4 system is intended to provide.

## 5. Why Y61 won the trade

The Y61 is heavier than Q610 but buys more than convenience.

Approximate core mass comparison:

| Item | Q610 concept | Y61 concept |
|---|---:|---:|
| eAxle / motor-diff assembly | ~91 lb | ~133 lb community teardown benchmark |
| dedicated/usable inverter | solution-dependent | ~20 lb OEM rear inverter benchmark |
| rear launch thrust | ~889 lbf | ~947 lbf |
| nameplate power | ~40 kW | ~70 kW |
| full-thrust region | ~0–21.5 mph | ~0–35 mph |
| standalone control burden | high today | low/moderate; existing CAN ecosystem |

The Y61 therefore trades roughly ~40 lb of eAxle mass for:

- slightly more launch thrust;
- ~30 kW more motor capability;
- much stronger 30–60 mph rear-force persistence;
- OEM-matched resolver and current control;
- OEM field weakening and motor protection;
- known CAN torque/regen control precedent;
- lower risk of spending months reproducing an OEM motor controller.

This is an intentional system-engineering trade. The project prefers to spend custom engineering effort on Celica-specific problems rather than on basic PMSM control.

## 6. Battery determination — Ford C-Max Hybrid remains selected reference

The Ford C-Max **Hybrid, non-Energi** 1.4-kWh pack remains unusually well matched to the architecture.

DOE/INL baseline test data for a 2013 pack documents:

- 76 series cells;
- 281.2 V nominal;
- 5.0 Ah;
- 1.4 kWh;
- 76 lb pack mass;
- active forced-air cooling;
- 10-second discharge power capability at 50% DOD: **66.0 kW**;
- 10-second charge power capability at 50% DOD: **47.8 kW**.

This confirms the design philosophy: the system is constrained by short-duration power, not energy capacity.

A 70 kW Y61 does **not** mean the battery must be enlarged until it can sustain 70 kW indefinitely. The BMS remains authoritative and the VCU must limit requested motor torque to the pack's instantaneous discharge/regen capability.

At the tested 66 kW DC pulse level, expected shaft/wheel power will be somewhat below the motor's 70 kW nameplate after inverter/motor/mechanical losses. That is acceptable. The value of the Y61 is its available torque and its ability to use whatever battery power is presently permitted without requiring a larger traction battery merely to justify the motor.

Battery strategy remains:

- retain the complete OEM pack/enclosure initially;
- retain Ford BECM/cell monitoring/current/temp/contactors/cooling if standalone CAN control is practical;
- VCU consumes Ford charge/discharge limits and never overrides them;
- prefer a mechanically failed but electrically healthy complete C-Max Hybrid as a future reference donor if obtainable cheaply.

## 7. Speed / disconnect determination

Stock Y61 gearing is intentionally retained because it is a major part of the performance benefit.

With 235/35R18 and 7.065:1:

| Vehicle speed | Y61 motor speed |
|---:|---:|
| 60 mph | ~5,820 rpm |
| 80 mph | ~7,760 rpm |
| 90 mph | ~8,730 rpm |
| 95 mph | ~9,220 rpm |
| 100 mph | ~9,700 rpm |

Community material places the Y61 rear motor near the ~9,600–10,000 rpm neighborhood. Treat the exact mechanical/electrical limit as **TENTATIVE until verified for the acquired donor**.

Current safety rule:

> Until a mechanical disconnect is designed and validated, the vehicle gets a conservative software vehicle-speed limit of approximately 90 mph.

This means early street development, launches and autocross are feasible, but normal high-speed track use is not.

### Preferred disconnect concept

Current preferred architecture is an **external one-side inboard halfshaft disconnect used with the open differential**.

Desired hardware:

- bearing-supported inboard shaft assembly;
- splined sliding dog clutch;
- electrically actuated eventually;
- positive engaged/disengaged sensing;
- serviceable independently of the Y61;
- rear torque forced to zero whenever state is unresolved.

The attraction is that the disconnect sees axle-side high torque but comparatively low rotational speed rather than living inside the five-digit-rpm motor/reduction assembly.

### Synchronization concept

Reconnect:

1. command rear torque to zero;
2. calculate target differential/motor speed from wheel speeds;
3. use the Y61 motor to synchronize the two sides of the disconnect;
4. verify small delta-RPM;
5. engage dog clutch;
6. verify engagement;
7. ramp rear torque back in.

Disconnect:

1. ramp rear torque to zero;
2. verify near-zero transmitted torque;
3. release dog clutch;
4. allow motor/carrier to coast down.

### Staged disconnect development

Do not jump directly to 90-mph dynamic engagement.

- **Rev 0:** manual/stationary service disconnect concept; prove open-diff kinematics and lubrication.
- **Rev 1:** electrically actuated only while stationary.
- **Rev 2:** low-speed motor synchronization and engagement.
- **Rev 3:** dynamic disconnect/reconnect with progressively expanded speed envelope.

Important open calculation: with one halfshaft disconnected, verify differential side-gear/pinion relative speed and lubrication. Protecting the motor from overspeed does not automatically prove that the open differential is happy at the resulting internal relative speed.

## 8. Straight-line performance interpretation

The first-pass traction model should be used for ranking architectures, not predicting time slips.

Under optimistic assumptions (perfect front traction control, synthetic ~500-whp 2ZZ curve, ideal rear torque control, simplified vehicle mass/drag/shift behavior), a ~70 kW / ~950 lbf rear system produced theoretical performance in roughly:

- sub-4-second 0–60 territory;
- approximately mid/high-11-second quarter-mile territory.

Those numbers are not promises. Their significance is that the Y61-class system crosses the project's mental performance benchmark **without** requiring a 100–250 kW EV drive unit.

The more robust conclusion is the force envelope:

- Y61 launches with about ~950 lbf rear tire thrust;
- it can carry approximately that full thrust through the mid-30-mph region;
- above that, rear force tapers approximately with P/v while the ~500-whp front drivetrain becomes progressively more capable of using its own power.

This is the desired behavior.

## 9. Cornering and low-mu benefits

P4 is not only a launch device.

### Corner exit / friction-circle benefit

The front tires of a FWD car must produce both lateral and longitudinal force. Approximate combined-tire demand is bounded by:

```text
Fx^2 + Fy^2 <= (mu * N)^2
```

Rear propulsion can therefore increase total vehicle acceleration in a corner even when the front tires are not visibly spinning: every unit of longitudinal force moved rearward leaves more front friction capacity available for steering.

The Y61 uses one motor and an open rear differential; it is **not** a true left/right torque-vectoring axle. Rev-0 controls should focus on front/rear torque allocation.

Initial cornering logic should be conservative:

- corner entry: rear propulsion near zero;
- mid-corner: limited rear torque, bounded by lateral acceleration/yaw/slip;
- corner exit: increase rear torque as steering unwinds and rear longitudinal capacity returns;
- oversteer/yaw fault tendency: rapidly reduce positive rear torque toward zero;
- do not begin with clever corrective negative rear torque.

A yaw-rate/lateral-acceleration IMU and steering-angle input become valuable later, but are not required for the first straight-line POC.

### Rain / low-mu benefit

As mu falls, the rear force required to use all available tire traction falls as well. Therefore a ~950 lbf Y61 axle becomes proportionally more capable in rain/snow than it is on a dry high-grip surface.

Low-mu calibration priorities:

- gentle torque slew rates;
- wheel-slip limits;
- stability before maximum acceleration;
- BMS/thermal limits remain authoritative;
- no direct driver-to-inverter torque path.

## 10. VCU architecture

The final car should have a dedicated VCU between the Celica/EMU systems and the Mitsubishi/Ford hardware.

### EMU / front-drivetrain inputs

- accelerator / requested engine torque;
- engine RPM;
- throttle position;
- MAP/boost/load as useful;
- clutch switch/state;
- gear or derived gear;
- engine operating/fault state.

### Chassis inputs

- four wheel speeds;
- brake switch / brake pressure if added;
- vehicle speed;
- disconnect engaged/disengaged sensing;
- later: steering angle, yaw rate and lateral acceleration.

### Ford battery inputs

- SOC;
- pack voltage/current;
- maximum discharge current/power;
- maximum charge/regen current/power;
- temperatures;
- contactor/precharge state;
- faults.

### Mitsubishi inverter interface

VCU command should fundamentally be:

```text
requested rear motor torque
```

Feedback should include as available:

- motor RPM;
- actual torque/current;
- DC voltage/current;
- motor temperature;
- inverter temperature;
- inverter ready/fault state.

### Operating modes

- zero-torque standby;
- launch assist;
- front-slip reduction / rear torque addition;
- off-boost torque fill;
- shift fill;
- mild lift regen;
- brake-coordinated regen;
- push-button stronger regen/charge request;
- through-the-road cruise charging;
- disconnect synchronization/engagement;
- thermal/SOC/power derating;
- immediate fault inhibit;
- later corner-exit and low-mu traction allocation.

The regen button is a **request to the VCU**, never a hardwired inverter command.

## 11. Preliminary cost and mass gates

Current planning, not purchase authorization:

### Y61 bench / hardware budget

A realistic bench-running propulsion POC is roughly **$3,000–$4,500**, depending heavily on salvage pricing and how much useful cabling/connectors come with the donor parts.

Expected major items:

- Y61 rear motor/differential;
- matched OEM rear inverter;
- donor motor/inverter HV cable and pigtails;
- C-Max Hybrid pack/reference donor;
- VCU/CAN hardware;
- HV fuse/contactors/precharge/service disconnect;
- cooling loop;
- bench fixture and safety hardware.

### Installed project gate

Current provisional target:

```text
finished DIY cost: preferably <= ~$6,000, tolerate roughly $5k–$7k planning band
net vehicle mass addition: target <= ~275 lb, investigate up to ~300 lb before rejecting
```

Core Y61 mass is the principal disadvantage:

- motor/diff/bracket community teardown benchmark: ~133 lb;
- OEM rear inverter benchmark: ~20 lb;
- C-Max battery: 76 lb;
- core three pieces: ~229 lb before mounting, HV, cooling, axles, disconnect and cradle delta.

Therefore a complete Y61 system likely lands in the rough **270–300 lb gross-added** region until actual CAD/weighing proves better. This is the most important packaging/mass gate.

Do not enlarge the battery merely because the Y61 is rated at 70 kW.

## 12. Current decision record

### DEC-SIDE-EAWD-001 — use P4 / through-the-road rear drive

**Selected.** Turbo 2ZZ + E153 remain mechanically independent of the rear electric axle.

### DEC-SIDE-EAWD-002 — select Y61 + OEM rear inverter as current drivetrain baseline

**Selected current direction.** The added mass is accepted provisionally because it buys the desired ~950 lbf / 70 kW force envelope and avoids custom PMSM-control development.

### DEC-SIDE-EAWD-003 — retain C-Max Hybrid non-Energi pack as battery reference

**Selected current direction.** Its 76-lb, 281.2-V, 1.4-kWh, high-pulse-power architecture matches the torque-buffer mission unusually well.

### DEC-SIDE-EAWD-004 — retain stock Y61 gearing and pursue disconnect rather than regear

**Selected current direction.** Aggressive 7.065 gearing is a feature at launch. Do not sacrifice it solely to accommodate road speeds where rear assistance has low value.

### DEC-SIDE-EAWD-005 — Q610 is watch-list optimization, not present development path

**Selected hold.** Reconsider when inexpensive, documented standalone torque/regen control exists or if Y61 packaging/mass fails.

### DEC-SIDE-EAWD-006 — Q211 custom-regear architecture superseded

**Superseded.** The regear solves overspeed but sacrifices too much rear thrust and adds custom-gear manufacturing burden.

## 13. Current kill / reconsider gates

Reconsider the Y61 baseline if any of the following prove true:

- Y61 output-center/package geometry cannot coexist with acceptable Celica/Matrix suspension, fuel-tank and floor geometry;
- complete installed mass cannot reasonably remain around <=300 lb;
- OEM rear inverter control proves materially less mature/reliable than current community evidence suggests;
- C-Max pack/BECM standalone control becomes disproportionately difficult or pack condition/availability makes a different lightweight HEV battery clearly superior;
- one-side disconnect kinematics create unacceptable differential speed/lubrication/durability problems;
- total cost moves materially above the ~$5k–$7k grassroots planning band without delivering corresponding capability.

If Y61 fails primarily on mass/packaging, Q610 becomes the first architecture to revisit.

## 14. Development philosophy

Baby steps. Development should occur almost entirely off-car.

The interesting Celica-specific work is:

- chassis/package integration;
- battery supervisory interface;
- VCU torque allocation;
- launch/shift/corner/weather control;
- synchronized disconnect;
- HV safety and fault behavior.

Avoid taking ownership of mature OEM problems unless necessary. In particular, prefer the Y61 OEM inverter so the project asks **what rear torque should the Celica command?**, not **how do we make an unfamiliar PMSM produce torque at all?**

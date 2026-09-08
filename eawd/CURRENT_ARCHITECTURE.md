# Celica P4 e-AWD — Current Architecture

**Checkpoint:** 2026-09-08  
**Status:** parked / documented concept  
**Current baseline:** Mitsubishi Y61 rear drive + matched OEM rear inverter + Ford C-Max Hybrid non-Energi battery  
**Authoritative trade record:** [`TRADE_STUDY_2026-09-08.md`](TRADE_STUDY_2026-09-08.md)  
**Legacy architecture record:** [`PROJECT.md`](PROJECT.md) preserves the superseded Q211 / Gen-3 Prius-inverter / custom-regear concept and its useful battery-CAN research.

## Mission

Retain the turbo 2ZZ + E153 as the primary front drivetrain and add an independent P4 electric rear axle. The system exists to use traction the front axle cannot efficiently use, not to turn the Celica into an EV.

Desired functions:

- AWD launch traction;
- off-boost torque fill;
- shift fill while the clutch is open;
- corner-exit front-tire relief;
- rain / low-mu traction allocation;
- modest midrange assistance;
- mild lift/brake regen;
- optional driver-requested stronger regen;
- optional through-the-road cruise charging.

## Selected architecture

```text
                         FRONT

 turbo 2ZZ -> E153 LSD -> front wheels
      |
      | torque / rpm / throttle / clutch / faults
      v
    Celica VCU <------------------------------+
      |                                       |
      | rear torque request                   | battery limits / SOC / faults
      v                                       |
 Mitsubishi OEM rear inverter <------ Ford C-Max Hybrid BECM / battery
      |
      | three phase
      v
 Mitsubishi Y61 rear motor / reduction / open diff
      |
      +--> inboard synchronized one-side disconnect (future)
      |
 rear CVs / custom axle bars as required
      |
 Matrix/Celica-compatible hubs / suspension geometry
```

## Core hardware

| Component | Current baseline | Planning note |
|---|---|---|
| Rear drive | Mitsubishi Y61, later Outlander PHEV rear unit | ~70 kW / ~195 Nm / 7.065:1; ~133 lb community teardown benchmark including diff/brackets |
| Inverter | Matched Mitsubishi OEM rear inverter | ~20 lb benchmark; retain OEM motor control and command torque over CAN |
| Battery | Ford C-Max Hybrid, non-Energi | 281.2 V nominal, 1.4 kWh, 76 lb; DOE-tested high pulse power |
| VCU | Dedicated Celica hybrid VCU | Translates EMU/chassis/BMS state into safe requested rear torque |
| Rear differential | OEM open diff | Initial architecture does not require rear LSD or twin motors |
| Disconnect | One-side inboard dog-clutch concept | Manual/stationary first; synchronized electrical actuation later |

## Why Y61

The rear system was sized from required tire force, not donor nameplate power.

Using 235/35R18, 95% driveline efficiency and published/community Y61 torque/ratio values:

```text
rear axle torque ~= 195 * 7.065 * 0.95 ~= 1,309 Nm
rear tire thrust ~= 947 lbf
```

The ~70 kW motor can hold approximately that force through the mid-30-mph region before becoming power-limited. This is a strong fit for a high-power FWD Celica: it materially improves launch and 25–60 mph traction without requiring a 100–250 kW EV drive unit.

The Y61 was chosen over the lighter Toyota Q610 because the Mitsubishi OEM inverter already has a mature conversion-control ecosystem. The project intentionally accepts roughly ~40 lb more eAxle mass to avoid taking ownership of resolver calibration, FOC tuning, field weakening and motor characterization merely to make the rear motor produce torque.

See the trade study for full Q610/Q211 calculations and decision rationale.

## Battery philosophy

The battery remains intentionally tiny. DOE/INL testing of the 2013 C-Max Hybrid reference pack documents 66.0 kW 10-second discharge capability and 47.8 kW 10-second charge capability at 50% DOD.

Therefore:

- do not enlarge the battery merely to justify the Y61's 70 kW motor rating;
- use the Ford BECM's instantaneous charge/discharge limits as authoritative;
- VCU torque request must be clipped by battery, inverter, motor, thermal, slip and disconnect limits;
- preserve the complete Ford enclosure/BECM/contactors/service disconnect/cooling initially if standalone control is practical.

The legacy `PROJECT.md` contains the detailed C-Max BECM/CAN reverse-engineering workflow and remains valid for that subsystem unless later evidence supersedes it.

## Stock gearing and speed limit

Stock 7.065:1 gearing is retained intentionally because the launch multiplication is a feature.

Calculated Y61 rotor speed on 235/35R18:

| Vehicle speed | Motor speed |
|---:|---:|
| 60 mph | ~5,820 rpm |
| 80 mph | ~7,760 rpm |
| 90 mph | ~8,730 rpm |
| 95 mph | ~9,220 rpm |
| 100 mph | ~9,700 rpm |

Until the actual donor's safe mechanical/electrical speed limit is verified and the disconnect is validated:

> **Vehicle speed is software-limited to approximately 90 mph.**

This permits street POC, launches and autocross but not unrestricted track use.

## Disconnect development

Preferred concept: external one-side inboard halfshaft disconnect using the open differential.

Development sequence:

1. manual / stationary disconnect;
2. verify open-differential kinematics, carrier/motor behavior, internal relative speed and lubrication;
3. electrically actuated stationary dog clutch with positive position sensing;
4. low-speed motor-synchronized reconnection;
5. progressively expand dynamic disconnect/reconnect speed;
6. remove the temporary vehicle-speed limit only after verified operation and fault handling.

Rear torque must be zero during any unresolved disconnect state.

## Controls development — baby steps

Rev-0 needs only safe commanded rear torque in simple straight-line conditions.

Later functions can be layered in independently:

- launch assist;
- front-slip reduction;
- off-boost fill;
- clutch-open shift fill;
- lift/brake regen;
- driver-requested stronger regen;
- through-road charging;
- low-mu traction allocation;
- corner-exit front-tire relief;
- yaw/stability-aware rear torque reduction;
- disconnect synchronization.

Do not begin by trying to reproduce a complete OEM AWD/stability system.

## Cornering principle

P4 helps even when the front tires are not visibly spinning because the front tires must share lateral and longitudinal friction capacity:

```text
Fx^2 + Fy^2 <= (mu*N)^2
```

Moving some propulsion rearward leaves more front-tire capacity for steering. The first implementation is front/rear torque allocation only; the Y61/open-diff architecture is not true left/right torque vectoring.

Later useful chassis signals:

- four wheel speeds;
- steering angle;
- yaw rate;
- lateral acceleration;
- brake state/pressure.

Corner entry should initially use little/no positive rear torque; corner-exit torque can increase as steering unwinds. Oversteer tendency should rapidly reduce positive rear torque rather than attempt clever corrective regen in early revisions.

## Current planning gates

The project remains parked. Current planning targets are:

```text
complete installed mass: preferably <= ~275 lb; investigate up to ~300 lb before rejecting
finished DIY cost: preferably <= ~$6,000; rough planning band ~$5,000–$7,000
OEM fuel tank: retain if practical
car-down development: minimize; prove major systems off-car
```

The Y61 baseline should be reconsidered if:

- packaging around the Matrix/Celica hard points, tank or floor is unacceptable;
- installed mass cannot stay near/under ~300 lb;
- OEM inverter standalone control is materially worse than current evidence indicates;
- the C-Max BECM/battery becomes disproportionate work or a clearly better lightweight HEV pack exists when the project is revived;
- the one-side disconnect creates unacceptable differential speed/lubrication/durability issues;
- project cost grows materially outside the grassroots band.

If Y61 fails mainly on package or mass, Q610 is the first alternative to revisit.

## Current decision summary

- **DEC-SIDE-EAWD-001 — SELECTED:** P4 through-the-road rear drive; no mechanical front/rear coupling.
- **DEC-SIDE-EAWD-002 — SELECTED:** Mitsubishi Y61 + matched OEM rear inverter is the current rear-drive baseline.
- **DEC-SIDE-EAWD-003 — SELECTED:** Ford C-Max Hybrid non-Energi remains the battery reference.
- **DEC-SIDE-EAWD-004 — SELECTED:** retain stock Y61 gearing; solve high-speed operation with a staged disconnect.
- **DEC-SIDE-EAWD-005 — HOLD:** Q610 remains the lightweight future optimization if control becomes turnkey or Y61 fails package/mass gates.
- **DEC-SIDE-EAWD-006 — SUPERSEDED:** Q211 + custom ~4.07 regear is no longer the preferred final architecture.

## Near-term rule while parked

Do not buy a complete system merely because this architecture now looks promising.

The only passive sourcing interest should be unusually good donor opportunities for:

- 2021–2022-era Outlander PHEV Y61 rear drive unit;
- matched OEM rear traction inverter;
- factory Y61/inverter HV cables, pigtails, brackets and useful inner-CV hardware;
- mechanically failed but electrically healthy C-Max Hybrid non-Energi reference car/pack.

When the project is explicitly revived, the first real engineering gate is **Y61 packaging + mass characterization against the Matrix/Celica rear geometry**, followed by low-risk OEM-inverter CAN bench control.

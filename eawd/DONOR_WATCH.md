# Celica P4 e-AWD — Exact Donor Watch List

**Checkpoint:** 2026-09-09  
**Project state:** parked; opportunistic sourcing only  
**Current architecture:** Y61 + matched Mitsubishi OEM rear inverter + Ford C-Max Hybrid battery

## 1. Primary rear-drive donor — BUY/INVESTIGATE FIRST

### 2021–2022 Mitsubishi Outlander PHEV

**Make:** Mitsubishi  
**Model:** Outlander PHEV  
**Model years:** **2021–2022**  
**Trim:** any PHEV trim is acceptable; verify donor VIN/year and rear-drive hardware before purchase  

Why these years:

- 2021 introduced the upgraded **70 kW rear motor**;
- Mitsubishi identifies the rear motor code as **Y61**;
- published rear torque remains **195 Nm**;
- 2022 carries the upgraded 2021 powertrain forward;
- this is the exact generation currently used for the Celica performance calculations.

### Preferred same-donor parts

Pull/buy as one package whenever possible:

| Item | Mitsubishi reference | What to keep |
|---|---|---|
| Rear motor / reduction / open differential assembly | **Y61**, service motor assembly **9411A078** | Complete housing, reduction/diff, output stubs, mounts/brackets, fasteners |
| Rear drive-motor inverter | **9410A171** | Complete OEM rear inverter, covers/brackets, LV connector with generous pigtail |
| Rear motor wiring harness | **8556A131** | Keep complete if practical |
| Lower rear motor HV cable assembly | **9499D147** | Keep complete if practical; especially proprietary motor-side terminations |
| Rear inner CV / axle hardware | donor rear axles / inner joints | Keep both sides if inexpensive, even if custom axle bars are ultimately required |
| Inverter cooling interfaces | donor hoses/fittings/brackets | Useful for bench development and identifying OEM connection sizes |

### Acquisition preference

Best donor package:

> **2021 or 2022 Mitsubishi Outlander PHEV, rear motor/reduction/differential + rear inverter 9410A171 + motor/inverter wiring/cables + brackets + rear axle/inner-CV hardware from the same wrecked vehicle.**

Do not pay extra for the Outlander's traction battery; it is not the selected Celica battery architecture.

## 2. Secondary rear-drive donor — ONLY IF THE DEAL IS EXCEPTIONAL

### 2018–2020 Mitsubishi Outlander PHEV

**Make:** Mitsubishi  
**Model:** Outlander PHEV  
**Model years:** **2018–2020**  

These US-market cars also use a **Y61** rear motor and Mitsubishi publishes the same **195 Nm** peak torque, but the rear motor is rated **60 kW**, not the selected 70 kW envelope.

Use one only if:

- price is substantially better than a 2021–2022 package;
- the motor, **matched same-donor rear inverter**, cables and connectors come together;
- we intentionally accept the lower high-speed power envelope.

Do **not** assume a 2018–2020 inverter/calibration should be mixed with a 2021–2022 drivetrain merely because both motors are called Y61.

## 3. Do not confuse with the new-generation Outlander

### 2023+ Mitsubishi Outlander PHEV

The redesigned generation uses the newer **YA1** rear motor architecture, rated around **100 kW** in current Mitsubishi specifications.

That hardware is interesting future technology but is **not the current Celica baseline** because:

- it is more power than the present sizing study requires;
- standalone-control maturity is less attractive for this project;
- package/mass/electrical assumptions differ from the Y61 study.

Do not substitute a 2023+ rear drive unit into the Y61 design without reopening the trade study.

## 4. Battery / CAN reference donor

### 2013–2018 Ford C-Max Hybrid — NON-ENERGI ONLY

**Make:** Ford  
**Model:** C-Max Hybrid  
**Model years:** **2013–2018**  
**Exclude:** **C-Max Energi plug-in hybrid**

Selected battery concept:

- ~281 V nominal;
- ~1.4 kWh;
- ~76 lb benchmark;
- high-power HEV buffer rather than EV-energy pack;
- retain Ford BECM, sensing, contactors, service disconnect and cooling initially if standalone CAN control is practical.

### Preferred donor condition

Best acquisition is a cheap whole **2013–2018 Ford C-Max Hybrid** that:

- has a mechanical failure, especially transmission/drivetrain failure;
- still enters **READY**;
- has no HV-battery/BECM warning state;
- communicates normally with FORScan;
- has not sat dead/discharged for years.

Why a whole car can be valuable:

- intact BECM/CAN reference implementation;
- complete pack/enclosure/contactors/service disconnect/fan;
- all proprietary connectors and pigtails;
- ability to capture known-good wake/READY/discharge/regen/shutdown traffic.

### Year preference

- **2013:** strongest published DOE/INL test documentation; excellent reference donor.
- **2017–2018:** younger calendar age may be attractive for the actual pack, but verify pack/BECM revision and connector compatibility before assuming it is identical to the 2013 reference.

The **actual donor BECM's real-time charge/discharge limits are authoritative**. Do not design to the highest published test result.

## 5. Low-priority alternative watch

### Toyota Q610 / 4NM

Do not actively develop or buy a Q610 merely because one is cheap. It remains the first lightweight alternative only if:

- Y61 packaging/mass fails; or
- the conversion community demonstrates an inexpensive, documented, essentially turnkey Q610 torque + regen control solution.

If that happens, reopen the hardware/application list before purchasing; do not use stale donor-year assumptions from the superseded study.

## 6. Quick junkyard / Marketplace checklist

When a likely 2021–2022 Outlander PHEV appears, ask for or photograph:

1. VIN / model year;
2. complete rear motor/reduction/differential assembly;
3. rear inverter label — target **9410A171**;
4. rear motor assembly label / casting information — service reference **9411A078**;
5. rear motor harness **8556A131** if present;
6. lower motor cable **9499D147** if present;
7. both rear inner CVs / halfshafts;
8. motor and inverter brackets/fasteners;
9. inverter coolant fittings/short hose sections;
10. generous LV pigtails rather than connectors cut flush.

Prefer a complete same-donor package over individually sourced pieces even if the package costs modestly more.

## 7. Evidence references

Mitsubishi Motors North America 2021 Outlander PHEV press kit — Y61, 70 kW, 195 Nm:

https://media.mitsubishicars.com/en-US/releases/release-7984bdfc3f0deb5e744a8fe7aa00e70e-2021-outlander-phev

Mitsubishi Motors North America 2022 Outlander PHEV press kit — carries forward the upgraded 70 kW rear motor introduced for 2021:

https://media.mitsubishicars.com/en-US/releases/release-681cabca0fcb51bf8b4b21d77402736a-2022-outlander-phev

Mitsubishi Motors North America 2020 press kit — Y61, 60 kW, 195 Nm:

https://media.mitsubishicars.com/en-US/releases/release-c6017295472444d88b6f0d07ba36588f-2020-mitsubishi-outlander-phev-press-kit

Mitsubishi OEM parts catalog — 2021 rear motor assembly / harness references:

https://parts.mitsubishicars.com/v-2021-mitsubishi-outlander-phev--gt--2-4l-l4-electric-gas/electrical--body-wiring-harness-and-components

Mitsubishi OEM parts catalog — 2021 rear inverter 9410A171:

https://parts.mitsubishicars.com/v-2021-mitsubishi-outlander-phev--le--2-4l-l4-electric-gas/electrical--alternator-generator-and-related-components

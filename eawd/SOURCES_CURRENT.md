# Celica P4 e-AWD — Current Sources / Evidence Record

**Checkpoint:** 2026-09-08  
**Applies to:** current Y61 + OEM inverter + C-Max Hybrid architecture  
**Legacy source record:** [`SOURCES.md`](SOURCES.md) preserves Q211 / Prius-inverter research that remains useful if those alternatives are revisited.

## Mitsubishi Y61 rear drive

### 2021 Outlander PHEV owner documentation — motor rating

https://www.manualslib.com/manual/2195313/Mitsubishi-Motors-Outlander-Phev-2021.html?page=398

Useful published specification:

- rear motor model: **Y61**;
- maximum rear-motor output: **70 kW**;
- maximum rear-motor torque: **195 Nm**;
- maximum 30-minute rear-motor power: **30 kW**.

Treat the exact donor year/part number as a verification item because earlier Y61 applications were rated 50 or 60 kW.

### Mitsubishi service-manual mirror — rear transaxle ratio

https://mmc-manuals.ru/manuals/outlander_phev/online/Service_Manual_2014/2019/22/html/M122230010001300ENG.HTM

Useful factory-service information:

- rear transaxle model: F1E1A;
- motor model: Y61;
- parallel-axis mechanical reduction;
- total motor-to-differential ratio: **7.065:1**.

### OpenInverter — Mitsubishi Outlander Rear Drive Unit

https://openinverter.org/wiki/Mitsubishi_Outlander_Rear_Drive_Unit

Useful conversion-community baseline:

- Y61 application history and 50/60/70 kW variants;
- 195 Nm peak torque on later variants;
- 7.065:1 rear reduction;
- open rear differential / female driveshaft splines;
- documented use with both OpenInverter and the OEM Mitsubishi rear inverter;
- useful part-number and conversion references.

Community documentation is not a substitute for measuring/weighing the acquired donor.

## Mitsubishi OEM rear inverter / CAN

### OpenInverter — Mitsubishi Outlander Rear Inverter

https://openinverter.org/wiki/Mitsubishi_Outlander_Rear_Inverter

This is the primary evidence behind selecting the Mitsubishi OEM inverter instead of developing a custom motor controller.

The page documents a working CAN contract including:

- required heartbeat traffic;
- torque command frame;
- torque report;
- motor RPM report;
- HV report;
- motor temperatures;
- phase-current reporting;
- forward/reverse direction handling.

The documented torque-command range is approximately +/-200 Nm. This supports the architecture where the Celica VCU commands **requested rear motor torque** while Mitsubishi retains resolver/current/field-weakening control.

### ZombieVerter / OpenInverter ecosystem

https://openinverter.org/wiki/Zombieverter_Parameters_and_Spot_Values

Use as evidence that Outlander rear-inverter support is integrated into an existing conversion VCU ecosystem. The project does not have to use ZombieVerter as the final Celica VCU; the value is demonstrated standalone-control maturity.

## Y61 speed-limit evidence

### Community operating-speed reference

https://www.myoutlanderphev.com/threads/coasting-whats-happening.2006/

Community analysis cites a **10,000 rpm** rear-motor limit for the original Outlander architecture. Treat this as **TENTATIVE**, not factory design-release data.

For the Celica 235/35R18 tire and 7.065 ratio, calculations give approximately:

- 90 mph -> 8,730 rpm;
- 95 mph -> 9,220 rpm;
- 100 mph -> 9,700 rpm;
- 10,000 rpm -> ~103 mph.

The current ~90 mph pre-disconnect vehicle-speed limit is therefore intentionally conservative. Verify the exact donor's mechanical/electrical speed limit before expanding the envelope.

## Ford C-Max Hybrid battery

### U.S. DOE / INL — 2013 C-Max Hybrid VIN 5138

https://www.energy.gov/sites/default/files/2015/02/f19/batteryC-Max5138.pdf

Factory/literature and laboratory baseline:

- Panasonic lithium-ion;
- 76 cells in series;
- 281.2 V nominal;
- 5.0 Ah;
- 1.4 kWh;
- 76 lb complete pack benchmark;
- active forced-air cooling;
- **66.0 kW 10-second discharge capability at 50% DOD**;
- **47.8 kW 10-second charge capability at 50% DOD**.

### U.S. DOE / INL — 2013 C-Max Hybrid VIN 8698

https://www.energy.gov/sites/prod/files/2015/02/f19/batteryC-Max8698.pdf

A second tested vehicle/pack reported:

- **56.2 kW 10-second discharge capability at 50% DOD**;
- **42.2 kW 10-second charge capability at 50% DOD**.

This spread is important. Do **not** design the Celica around the best tested pack value. The BECM's real-time power limits and the actual donor pack condition are authoritative.

### Ford BECM / HEV diagnostics

https://www.fordservicecontent.com/ford_content/catalog/motorcraft/OBDSM1901_HEV.pdf

Useful evidence that the BECM supervises:

- cell voltages;
- pack temperatures;
- pack current;
- available charge/discharge power;
- cooling fan;
- HV junction-box contactors;
- network/fault behavior.

This supports retaining the Ford battery-management system and reverse-engineering only the minimum standalone message contract.

### C-Max community CAN research

https://github.com/cr08/Ford-C-Max-NA-Hybrid-PHEV-CAN-bus-research

Use as a signal-identification reference. Do not assume it provides a turnkey standalone READY/contactor sequence.

## Toyota Q610 alternative

The Q610 remains the first lightweight alternative to revisit if Y61 package/mass fails or community standalone control becomes turnkey.

Useful prior/current reference:

https://toyota-club.net/files/faq/19-10-10_faq_e-four_4wd_eng.htm

Project comparison values retained for trade calculations:

- approximately 40 kW;
- approximately 120 Nm;
- 10.781:1 reduction;
- approximately 41.1 kg / 91 lb module benchmark.

On the Celica tire, the first-pass model gives ~889 lbf rear thrust but only ~21.5 mph full-thrust/power crossover. The stock reduction also reaches 10,000 motor rpm at only ~67.5 mph.

## Toyota Q211 legacy alternative

See [`SOURCES.md`](SOURCES.md) for the detailed Q211/OpenInverter/Gen-3 Prius references.

Current retained comparison values:

- ~50 kW;
- ~130 Nm;
- 6.859:1 stock ratio;
- ~41.8 kg / 92 lb;
- ~613 lbf rear thrust on 235/35R18 with 95% assumed driveline efficiency.

The former ~4.072 custom-regear concept would reduce calculated rear thrust to ~364 lbf. That trade is now considered inconsistent with the project's launch-traction mission.

## Evidence hierarchy / current rules

- Manufacturer / factory-service / DOE evidence outranks forum/community evidence.
- OpenInverter evidence is highly valuable for conversion feasibility and CAN behavior but does not define Toyota/Mitsubishi mechanical design-release limits.
- The Y61 10,000-rpm limit remains **TENTATIVE** until better source material or controlled donor validation exists.
- Motor, inverter, battery, cable, bracket and cradle masses must be physically weighed for the actual parts acquired.
- Do not assume a 70 kW Y61 means the C-Max pack should be enlarged. Use the actual Ford BECM discharge/charge limits.
- Do not remove the temporary speed limit until disconnect operation, internal differential speed/lubrication and fault handling are validated.

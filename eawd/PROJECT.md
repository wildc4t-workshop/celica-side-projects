# Celica P4 e-AWD / Hybrid Rear Axle — Resume Here

## Project State

**Status:** parked / documented concept  
**Dashboard status:** one passive donor-watch task only (`SIDE-EAWD-001`); no execution work  
**Target vehicle:** 2000 Toyota Celica GT-S, turbo 2ZZ + E153 manual FWD drivetrain  
**Concept:** add an electrically driven rear axle without mechanically coupling the front and rear drivetrains  
**Rough DIY budget:** target about **$5,000**, with a planning range of roughly **$3,500–$7,000** depending mainly on custom gears, axles, and fabrication  
**Primary next action when/if revived:** acquire and characterize a cheap Toyota/Lexus Q211 rear motor-generator assembly; do not buy the rest of the system until packaging looks credible

This project is intentionally preserved now so it can be resumed years later without reconstructing the architecture from chat history. It is **not** part of Baseline or the committed Street Build and should not compete with current work. `SIDE-EAWD-001` exists only as a low-priority sourcing/watchlist reminder; do not create additional e-AWD tasks until the project is explicitly revived.

## Purpose

Add a compact P4-style electric rear axle to the Celica while retaining:

- the turbo 2ZZ and E153 manual transaxle driving the front wheels;
- the OEM gasoline fuel tank if packaging allows;
- normal gasoline-only drivability if the rear system is disabled;
- a street-car rather than EV-conversion operating model.

The rear system is primarily a **power buffer / torque-fill system**, not an EV-range system.

Desired effects:

- AWD launch traction;
- off-boost torque fill;
- rear torque during manual-transmission shifts while the front clutch is open;
- reduced front-tire traction burden;
- mild automatic lift-off regen;
- optional stronger driver-requested regen/charging;
- optional through-the-road charging, where the 2ZZ is deliberately asked for additional cruise torque while the rear motor regenerates enough to maintain road speed.

The objective is not to add a nominal “67 hp” and call the job done. The value is applying rear torque where the FWD car is weakest.

## Current Architecture

```text
                         FRONT

 turbo 2ZZ  ->  E153 LSD  ->  front wheels
       |
       | vehicle state / engine torque / throttle / rpm
       v
     HYBRID VCU
       |          ^
       | torque   | battery limits / SOC / faults
       v          |
 Gen-3 Prius inverter  <----  C-Max Hybrid battery
       |
       | three phase
       v
 Toyota/Lexus Q211 MGR
       |
  custom ~4.07:1 ratio
       |
   open differential
      / \
 rear CVs / axles
    /       \
 rear hubs / wheels

                         REAR
```

### Core hardware target

| Component | Current candidate | Planning mass | Why |
|---|---|---:|---|
| Rear drive unit | Toyota/Lexus Q211 MGR / 2FM | ~92 lb | Integrated ~50 kW PM motor, reduction and differential in a compact OEM assembly |
| Battery | Ford C-Max **Hybrid**, non-Energi | ~76 lb benchmark | Very small high-power HEV pack; OEM enclosure, sensing, contactors and forced-air cooling |
| Inverter | 2010–2015 Gen-3 Toyota Prius PCU | ~29 lb bare benchmark / allow ~30–40 lb installed | Compact, plentiful, open-source standalone-control path, power stage suitable for Q211-class output |

The three major components are therefore roughly **200–210 lb** before wiring, cooling, mounting, axles and controls. A preliminary whole-system target is roughly **225–250 lb net added mass**, recognizing that cradle parts, rear hardware, rear-seat/spare-tire changes, and actual installed accessories must be measured rather than assumed.

## Why the Q211

The Q211 is Toyota/Lexus's Motor Generator Rear (MGR) transaxle used for electric AWD assistance in vehicles such as the RX400h.

Known published/community specifications:

- motor: Toyota 2FM permanent-magnet AC machine;
- peak output: approximately **50 kW**;
- peak torque: approximately **130 Nm** (some sources list 139 Nm variants);
- layout: three parallel shafts;
- OEM overall reduction: approximately **6.859:1**;
- first reduction: **23T motor gear -> 40T counter gear**;
- final reduction: **18T pinion -> 71T differential ring gear**;
- lubricant: ATF WS, approximately 1.8 L;
- complete module mass: approximately **41.8 kg / 92 lb**;
- open differential;
- removable differential/output stubs make axle adaptation comparatively approachable.

The RX400h is the preferred junkyard donor because the Q211 application is well established and the rear drivetrain parts can be harvested together.

## The Overspeed Problem and Preferred Regear

### Why the stock ratio is wrong for this Celica

The Q211 was designed around large-diameter hybrid-SUV tires and a limited vehicle-speed envelope. With the Celica's assumed **235/35R18** tire, a stock 6.859 ratio spins the motor to roughly **10,000 rpm at only ~106 mph**.

Commanding zero electrical torque does not solve this: a permanent-magnet rotor is still mechanically back-driven by the wheels.

Earlier disconnect concepts included an axle disconnect or internal dog clutch. Those remain fallback ideas, but they add moving parts and control complexity.

### Preferred direction: replace only the first reduction pair

The current preferred concept is approximately:

- **31T motor gear -> 32T counter gear**;
- retain the OEM **18T -> 71T** final stage.

This produces:

```text
first stage = 32 / 31 = 1.0323
final stage = 71 / 18 = 3.9444
overall = 1.0323 x 3.9444 ~= 4.072:1
```

The OEM first-stage tooth-count sum is 23 + 40 = **63**. A 31 + 32 pair also totals **63 teeth**, so with compatible module/helix geometry the pair can preserve the existing shaft center distance while radically changing the ratio. The individual gear pitch diameters necessarily change; the design does **not** preserve each OEM gear's original diameter.

The 31/32 tooth counts are also coprime, producing a hunting-tooth relationship rather than repeatedly pairing the same teeth.

This is a **preliminary gear-design direction, not a released design**. Actual work must establish:

- normal/transverse module;
- helix angle and hand;
- pressure angle;
- face width;
- profile shift;
- shaft/hub/spline geometry;
- material and heat treatment;
- backlash/contact pattern;
- case clearances;
- bearing loads;
- lubrication/splash implications;
- manufacturability and cost.

### Resulting speed relationship

Using the assumed 235/35R18 tire:

| Vehicle speed | Stock 6.859 Q211 | ~4.072 regear |
|---:|---:|---:|
| 60 mph | ~5,650 rpm | ~3,350 rpm |
| 80 mph | ~7,540 rpm | ~4,470 rpm |
| 100 mph | ~9,430 rpm | ~5,590 rpm |
| 150 mph | ~14,140 rpm | ~8,380 rpm |
| ~179 mph | ~16,870 rpm | ~10,000 rpm |

A **10,000 rpm Q211 mechanical limit is currently a design assumption based on published/community material and must be verified before release**.

### Torque trade

At 130 Nm motor torque:

```text
stock axle torque ~= 130 x 6.859 = 892 Nm before losses
regeared axle torque ~= 130 x 4.072 = 529 Nm before losses
```

After reasonable drivetrain loss, the preliminary rear-tire thrust estimate is around **350–365 lbf**, roughly **0.12 g** of additional low-speed acceleration on a ~3,000 lb car.

This deliberately trades extreme very-low-speed multiplication for useful rear drive over the Celica's entire realistic speed range.

## Why Regearing Also Helps the Electrical Architecture

At a given road speed, the ~4.072 ratio spins the Q211 at only:

```text
4.072 / 6.859 ~= 0.594
```

or about **59% of the stock motor speed**.

Motor back-EMF scales approximately with motor speed, so the voltage requirement at a given vehicle speed falls substantially as well. This is why the low-voltage-for-an-OEM-hybrid **~281 V C-Max pack** becomes much more attractive when paired with the regear.

The current baseline is therefore:

```text
C-Max Hybrid pack (~281 V nominal)
        |
        | direct propulsion DC bus
        v
Gen-3 Prius inverter MG stage
        |
        v
regeared Q211
```

The Prius boost converter is **not** part of the baseline propulsion path. ORNL benchmarking of the 2010 Prius identifies the boost converter as roughly 27 kW, below the Q211's ~50 kW motor rating. The intent is to use the Gen-3 inverter power stage directly and let the regear reduce the motor-speed/back-EMF problem.

**Open verification item:** bench testing must determine the actual Q211 torque/power envelope from a ~281 V pack. Do not assume 50 kW is available to maximum vehicle speed. Full rear power at very high road speed is not a core requirement; low/mid-speed traction and torque fill are.

## Battery — Ford C-Max Hybrid

### Benchmark pack

DOE testing of a 2013 C-Max Hybrid documents:

- Panasonic lithium-ion battery;
- **76 cells in series**;
- **281.2 V nominal**;
- **5.0 Ah** rated capacity;
- **1.4 kWh** rated energy;
- **76 lb** pack mass;
- active forced-air cooling;
- measured baseline 10-second power capability around **56.2 kW discharge / 42.2 kW charge at 50% DOD** on one tested vehicle.

That is almost ideal for this project: very little energy, but a large amount of short-duration power.

### Donor-year strategy

The 2013 pack is the best-documented benchmark. Later C-Max Hybrid years continued using a nominal 1.4 kWh lithium-ion HEV battery system. If buying a later 2017–2018 donor for reduced calendar age, verify the exact pack revision, connector/BECM compatibility and physical dimensions before assuming interchange.

**Do not confuse the Hybrid with the C-Max Energi.** The Energi battery is a much larger energy-storage system and is not the preferred weight/packaging solution.

### Packaging direction

The complete Hybrid battery is compact enough that a **trunk installation is preferred if a safe structural mount and cooling-air path fit**. Rear-seat-delete space remains a fallback rather than a requirement.

Keep the complete OEM enclosure initially. The large Ford cooling blower/ducting may be repackaged later only after logging battery temperature and fan demand establishes the real airflow requirement.

Prefer conditioned cabin/trunk intake air rather than hot/wet underbody air.

## Preferred Battery Acquisition Strategy: Whole Dead C-Max

A loose complete pack is acceptable, but the most useful future donor may be a mechanically failed C-Max Hybrid that still powers up normally.

Ideal donor profile:

- 2013–2018 C-Max Hybrid, **not Energi**;
- severe HF35 transmission problem or other mechanical issue that destroys resale value;
- still enters **READY**;
- no hybrid/battery warnings;
- BECM communicates normally through FORScan;
- recently driven rather than abandoned with a discharged pack for years.

Why buy the whole car if cheap enough:

- unlimited CAN capture from an intact reference implementation;
- complete battery/BECM/contactors/service disconnect/fan;
- all proprietary pigtails, HV cables and harness sections;
- exact mounting and duct geometry to measure;
- ability to return to the original car whenever a CAN hypothesis needs testing;
- potential to resell useful parts and scrap the shell afterward.

Any part-out profit is a bonus, **not part of the engineering justification**. Underwrite the donor as acceptable even with conservative recovery.

## C-Max Battery CAN Reverse-Engineering Plan

The goal is **not** to reverse-engineer the entire C-Max. The goal is to preserve Ford's battery-management intelligence and learn the minimum message contract required to operate the BECM/Battery Energy Control Module outside the vehicle.

Ford's BECM already supervises the cells, temperatures, pack current, power capability, contactors and battery cooling. The VCU should consume Ford's limits rather than trying to become a second BMS.

### Data wanted from the battery

At minimum:

- SOC;
- pack voltage;
- pack current;
- available discharge power/current;
- available charge/regen power/current;
- battery temperatures;
- battery/fault state;
- contactor/precharge state;
- cooling/fan state or request if available.

### Messages likely required by the BECM

Determine experimentally rather than guessing:

- wake/run state;
- powertrain alive/heartbeat messages;
- READY/HV-enable state;
- any required rolling counters/checksums;
- shutdown state.

### Capture workflow

Use a functioning C-Max with two views of the same events:

```text
C-Max HS-CAN
   |
   +--> raw CAN logger / SavvyCAN
   |
   +--> FORScan named battery PIDs
```

Capture clean logs for:

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

Correlate named FORScan PIDs with raw frames to identify SOC, current, voltage, temperatures and power limits.

### Bench workflow

1. Start with the Ford battery **de-energized / service disconnect removed** and establish only low-voltage power, grounds, CAN and interlocks from factory documentation.
2. Observe what the BECM broadcasts on its own and what faults appear without the rest of the C-Max.
3. Replay captured vehicle traffic to the BECM.
4. Remove groups of CAN IDs until the minimum required set is isolated.
5. For required IDs, vary bytes/bits and identify state, rolling counters and checksums.
6. Replace replay with a minimal VCU state machine.
7. Only then proceed to controlled HV precharge/contactor testing using the OEM safety architecture.

The intended final architecture is:

```text
Celica VCU <---- CAN ----> Ford BECM
   |                         |
   |                         +--> cell monitoring
   |                         +--> temp/current sensing
   |                         +--> power limits
   |                         +--> precharge/contactors
   |                         +--> battery cooling
   |
   +--> rear torque request / regen limit
```

A manual regen button should be treated as a **driver request**, not hardwired motor torque. The VCU remains responsible for SOC, battery limits, wheel slip, speed and fault constraints.

## Inverter — Gen-3 Prius

Preferred donor: **2010–2015 Toyota Prius Gen 3** inverter/PCU with a compatible OpenInverter-style replacement logic board.

Why:

- ORNL measured the complete 2010 Prius PCU at approximately **13 kg / 29 lb**;
- large junkyard supply;
- open-source standalone-control precedent;
- MG2 power stage is in the correct ~50+ kW class;
- integrated liquid cooling;
- integrated HV-to-12 V DC/DC hardware may be useful later, though it is not required for the first architecture.

### Why not Gen-2 Prius

The 2004–2009 Gen-2 inverter is inexpensive and hackable, and Q211 operation with it has community precedent. It was rejected as the planned final component because the Gen-3 PCU is dramatically lighter and better matches the project's weight target. There is no reason to buy an intermediate inverter merely because one is nearby.

### Why not the RX400h inverter

The RX400h PCU is the OEM hardware designed around the Q211 and is useful as reference material, but it is large/heavy and has a less convenient standalone-control path. Do not buy it unless it is essentially free and wanted for research.

## Mechanical Integration

### Existing geometry donor

A complete Toyota Matrix AWD rear suspension/cradle assembly exists for future comparison. It should be treated as a **hard-point/geometry donor**, not proof that the Matrix subframe bolts into the Celica.

The assembly is not currently available for measurement. Do not design from remembered dimensions. When it becomes available, scan and measure it against the Celica and Q211.

### Cradle direction

Likely architecture:

- preserve the relevant Celica/Matrix suspension pickup geometry;
- use the existing rear assembly as a jig/reference;
- redraw or heavily modify the center cradle around the Q211;
- locate the Q211 primarily from output/CV geometry;
- retain the OEM fuel tank if practical;
- permit limited spare-tire/trunk-floor intrusion if that is cleaner than moving the fuel tank.

The Q211 has no longitudinal prop-shaft snout, so despite being physically larger than the tiny Matrix differential, its volume distribution may package more favorably around the Celica fuel tank.

### Axle strategy

Do not assume custom shafts until Toyota parts-bin compatibility is physically checked.

Try in this order:

1. test whether the Matrix AWD rear axle/inner CV interface fits the Q211/output-stub geometry directly;
2. if not, test whether Q211/RX inner CV components and Matrix outer CV components share a usable shaft interface;
3. if not, use **Q211/RX inner CV + custom axle bar + Matrix outer CV/hub**.

Custom axle bars are acceptable; the goal is to retain robust OEM CV interfaces at both ends.

Keep Q211 output stubs and RX inner-CV pieces with the junkyard motor even if they appear unnecessary at first.

## Q211 Lubrication / Thermal Strategy

The OEM Q211 uses gear-sling lubrication: the ring gear throws ATF into a catch/transfer path that gravity-feeds internal areas.

Changing the first-stage gear diameters could change splash behavior. Do **not** solve that problem before data exists.

Rev-0 strategy:

- preserve OEM ATF fill/drain paths;
- use the OEM stator-temperature sensor as the primary motor thermal metric;
- inspect the gear/catch geometry carefully during any regear teardown;
- add gear-oil temperature only if useful during validation.

If custom gearing or sustained regen creates a demonstrated lubrication/temperature problem, a simple future loop is plausible:

```text
drain/sump -> small pump -> compact cooler/filter -> return through fill area
```

The fill location appears favorable for returning oil over the gearset, but this must be verified on the actual unit.

## VCU / Vehicle Controls

The hybrid VCU is conceptually simple even though calibration will require care. Its primary output is **requested rear motor torque**, positive or negative.

Potential inputs:

- accelerator position / driver torque request;
- engine rpm and estimated engine torque from EMU Black;
- vehicle speed;
- individual wheel speeds;
- clutch state;
- brake state;
- gear if available/derived;
- Q211 speed and stator temperature;
- inverter current/voltage/temperature;
- Ford BECM SOC, charge/discharge limits and faults.

Desired modes:

- launch/AWD assist;
- off-boost torque fill;
- shift fill while the E153 clutch is open;
- rear torque trim when rear slip is detected;
- mild lift-off regen;
- stronger push-button regen/charge request;
- through-the-road charging during cruise when useful;
- zero-torque standby and full fault inhibit.

The electric rear axle should never be allowed to mask a battery, inverter, resolver, wheel-speed or thermal fault.

## Shopping List — Staged, Not All at Once

### Stage A — only purchase needed to answer the next question

**Toyota/Lexus Q211 MGR**, preferably from a 2006–2008 RX400h AWD donor.

Pull with it:

- complete Q211 housing/motor/differential;
- both output stubs;
- all factory Q211 mounting brackets and isolators;
- mounting hardware;
- resolver / low-voltage connectors with generous pigtails;
- temperature-sensor pigtail if separate;
- as much of the 3-phase HV cable/connector as practical;
- RX rear inner CVs or at least inner joint/stub pieces if cheap.

Target junkyard price: roughly **$100–300**, with ~$200 an easy experimental buy if condition is reasonable.

Then:

- weigh it;
- photograph every face;
- 3D scan it;
- hand-measure mounting/output datums;
- inspect oil/fill/drain/vent paths;
- store it until the Matrix assembly and vehicle geometry can be compared.

### Stage B — only after mechanical packaging looks credible

**2010–2015 Gen-3 Prius inverter/PCU** plus:

- compatible standalone/OpenInverter control board;
- low-voltage connector/pigtails;
- useful HV terminals/cable ends;
- coolant fittings and possibly donor pump if convenient.

Watch for a cheap unit; there is no urgency.

### Stage C — only after Q211/inverter bench control is credible

Preferred battery options, in order:

1. cheap mechanically dead but electrically alive **C-Max Hybrid donor car** for CAN/reference work, then retain its pack if appropriate;
2. complete tested C-Max Hybrid battery with BECM/contactors/service disconnect/fan/enclosure;
3. aftermarket BMS/supervisory electronics only if Ford standalone control proves impractical.

### Integration hardware later

- custom Q211 first-stage gear pair;
- rear axles/custom bars as required;
- custom/modified rear cradle;
- HV fuse and properly rated HV cable/connectors;
- inverter coolant pump, compact heat exchanger, hoses and reservoir/degassing provision;
- VCU and CAN interfaces;
- LV fuse/relay distribution;
- service disconnect/interlock provisions;
- battery structural mount and cooling ducting;
- guards/shields and service labels.

### Do not buy merely because it is available

- Gen-2 Prius inverter as an intermediate step;
- RX400h PCU unless nearly free and wanted as a research article;
- C-Max Energi modules for the current lightweight architecture;
- custom gears before the Q211 physically fits;
- a loose battery before there is a reason to start the Ford-CAN phase.

## Rough Cost Model

| Item | Planning range |
|---|---:|
| Q211 + useful donor hardware | $200–400 |
| C-Max Hybrid battery / effective donor cost | $400–1,000 |
| Gen-3 Prius inverter | $150–350 |
| Standalone inverter controller | $300–500 |
| CAN/dev electronics | $150–400 |
| Custom helical first-stage gear pair | $800–2,000+ |
| Axle solution | $500–1,200 |
| Cradle material/machining | $500–1,000 |
| HV cable/connectors/cooling/misc. | $500–1,000 |
| **DIY planning total** | **~$3,500–$7,000** |

The gear pair is currently the largest single cost/manufacturing uncertainty. A **$5k target** is reasonable but should not be treated as a hard cap.

## Development Sequence

The Celica should stay driveable for essentially the entire R&D process. Do development off-car and integrate only after the system is proven.

### Phase 0 — acquire/characterize Q211

- buy only the Q211 and donor interfaces;
- inspect, weigh, photograph, scan and measure;
- do not buy the battery because of enthusiasm.

### Phase 1 — mechanical feasibility

When the Matrix rear assembly is available:

- establish a common coordinate system;
- scan/measure Celica rear body, OEM tank, floor, exhaust corridor and suspension/body mounts;
- scan/measure Matrix rear suspension and subframe geometry;
- place Q211 by output-shaft/CV geometry;
- sweep suspension/axle motion;
- determine whether the OEM fuel tank can stay;
- define preliminary cradle architecture.

**Kill/rethink the project here if acceptable packaging requires destroying too much of the Celica.**

### Phase 2 — inverter + low-energy motor control

- acquire Gen-3 inverter/controller only after Phase 1 is promising;
- establish resolver phasing and Q211 control at low voltage/low torque;
- prove commanded forward/reverse torque and regen behavior on a safe fixture;
- characterize basic current, voltage, speed and temperature signals.

### Phase 3 — battery/CAN development

- acquire a C-Max Hybrid reference car or complete battery;
- capture Ford CAN/FORScan data;
- isolate BECM message contract;
- bench the battery using OEM BMS/contactors/precharge/interlocks;
- prove safe wake/READY/shutdown behavior before connecting the traction inverter.

### Phase 4 — integrated HV bench POC

- connect battery, inverter and Q211 with proper HV protection;
- begin at tightly limited torque/current/speed;
- prove motoring and regen;
- map battery/inverter/motor limits;
- determine the direct-281-V Q211 power envelope.

Do not chase unloaded full-power testing on an unsafe fixture.

### Phase 5 — regear engineering

- tear down a Q211 and fully characterize the OEM helical first-stage geometry;
- obtain professional gear-design/manufacturing input;
- release 31/32 or revised tooth-count solution only after case/bearing/contact/lubrication checks;
- bench-validate noise, temperature, lubrication and overspeed behavior.

### Phase 6 — vehicle integration

Only after the above closes:

- fabricate/finish cradle;
- finalize axles;
- mount battery/inverter;
- install HV/LV harnesses and cooling;
- integrate VCU with EMU Black/vehicle signals;
- begin with conservative torque and vehicle-speed limits;
- expand the operating envelope through logged validation.

## Principal Unknowns / Risks

### 1. Physical fit — first gate

Unknown until Q211, Matrix geometry and Celica underbody/tank are measured together.

### 2. Custom first-stage gears

Likely technically feasible but cost, manufacturing, helix geometry, case clearance, bearing load and lubrication effects are not yet closed.

### 3. Ford BECM standalone behavior

Expected to be conventional CAN reverse engineering rather than a fundamental obstacle, but the required wake/heartbeat/READY messages, counters and checksums must be proven.

### 4. Direct-bus voltage envelope

Regearing greatly reduces motor speed at a given road speed, but actual torque/power available from a ~281 V bus must be measured.

### 5. Q211 duty cycle / thermal behavior

Toyota designed it as an AWD-assist rear unit. Sustained custom duty, repeated regen and altered gearing may require additional cooling or derating.

### 6. Axle compatibility

Toyota parts-bin overlap may make this easy, but Matrix/Q211 spline interchange has not been physically verified.

### 7. Vehicle dynamics / control

Rear torque must be limited by wheel slip, speed, faults and battery capability. Open-diff behavior is acceptable for the first architecture; a custom LSD is not required.

## Current Decisions

**Selected / current direction:**

- P4 electric rear axle; no mechanical driveshaft from E153 to rear.
- Q211 as rear drive unit.
- Single motor + conventional/open differential, not twin rear motors.
- Preserve OEM fuel tank if practical.
- Prefer trunk battery placement if structurally and thermally clean; rear-seat-delete volume is fallback.
- Gen-3 Prius inverter for final lightweight architecture.
- C-Max Hybrid high-power/low-energy pack rather than a large PHEV/EV battery.
- Preserve Ford BMS/contactors if CAN work makes that practical.
- Regear Q211 rather than use a high-speed disconnect if the gear design is reasonable.
- Development off-car; vehicle integration last.

**Rejected / currently disfavored:**

- stock 6.859 Q211 gearing on Celica-sized tires for unrestricted road speed;
- one-way/sprag disconnect because it sacrifices regen;
- complex axle/motor disconnect as the primary solution while regear is viable;
- Gen-2 Prius inverter as a throwaway intermediate purchase;
- RX400h inverter as the planned final controller;
- C-Max Energi battery for this lightweight torque-buffer role;
- aftermarket LSD as an early requirement;
- overbuilding an external Q211 oiling system before temperature data exists.

## Resume Here — The Only Useful Near-Term Action

If the idea becomes interesting again before the rest of the project is ready:

> **Buy a cheap complete Q211 with its stubs, brackets, pigtails and useful CV/HV pieces. Scan it, measure it, and put it away.**

Do not buy a battery, commission gears, or turn the Celica into a fabrication project until Q211 packaging has earned the next dollar.

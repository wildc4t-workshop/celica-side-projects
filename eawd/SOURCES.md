# Celica P4 e-AWD — Sources / Research Record

## Purpose

Preserve the external evidence behind the parked P4 electric-rear-axle concept. These sources establish donor specifications and control precedent; they do **not** substitute for measuring, testing, or validating the actual parts acquired for the Celica.

## Toyota/Lexus Q211 MGR

### OpenInverter — Toyota/Lexus MGR Rear Transaxle Motor

https://openinverter.org/wiki/Toyota/Lexus_MGR_Rear_Transaxle_Motor

Useful published/community data for Q211-2FM:

- approximately 50 kW;
- approximately 130 Nm / some variants listed at 139 Nm;
- 6.859:1 overall ratio;
- first stage 23:40;
- final stage 18:71;
- three-shaft layout;
- approximately 1.8 L ATF WS.

Treat these values as a research baseline until verified against the actual donor unit.

### OpenInverter forum — MGR orientation / internal layout discussion

https://openinverter.org/forum/viewtopic.php?t=627

Useful because it preserves additional Q211 information including:

- approximately 41.8 kg module weight;
- gear-train identification;
- fluid-catch / sling-lubrication illustrations and discussion;
- application list including RX400h MHU38.

This is community documentation rather than Toyota design-release data.

## Gen-3 Prius inverter / PCU

### OpenInverter — Toyota Prius Gen3 Board

https://openinverter.org/wiki/Toyota_Prius_Gen3_Board

Establishes the current DIY-control precedent for repurposing **2010–2015 Prius** inverters by replacing the OEM logic board. The project exposes independent control of MG1, MG2, boost converter, and DC/DC functions.

The page describes the inverter as attractive for conversion use because of availability, durability, affordability, and power capability. Community figures include approximately 50 kW-class output and high MG2 current capability; verify any final operating limits against the actual board/inverter revision and controller documentation.

### ORNL — Evaluation of the 2010 Toyota Prius Hybrid Synergy Drive System

ORNL publication page:

https://www.ornl.gov/publication/evaluation-2010-toyota-prius-hybrid-synergy-drive-system-0

Report PDF:

https://info.ornl.gov/sites/publications/files/Pub26762.pdf

Report: ORNL/TM-2010/253, March 2011.

Useful benchmark data:

- complete 2010 Prius PCU mass approximately **13.0 kg**;
- complete PCU volume approximately 16.2 L;
- nominal Prius battery 201.6 V;
- maximum DC-link voltage 650 V;
- boost-converter power benchmark approximately **27 kW**;
- Gen-3 Prius MG2 rated 60 kW in the benchmark comparison.

This is the main rationale for preferring the Gen-3 PCU over the much heavier Gen-2 unit and for **not** treating the built-in boost converter as a 50 kW propulsion path from a ~281 V C-Max pack.

## Ford C-Max Hybrid battery

### U.S. DOE / Advanced Vehicle Testing — 2013 C-Max Hybrid baseline

https://www.energy.gov/sites/prod/files/2015/02/f19/fact2013fordc-maxhybrid.pdf

Useful battery data:

- Panasonic lithium-ion / NMC;
- 76 series cells;
- 3.7 V nominal cell voltage;
- **281.2 V nominal system voltage**;
- **5.0 Ah** rated capacity;
- **1.4 kWh** rated energy;
- **76 lb** pack mass;
- active fan cooling;
- pack location under the rear-seat/trunk-floor region in the test vehicle.

This is the primary dimensional/power-system benchmark for the proposed lightweight torque-buffer battery.

### U.S. DOE battery test — 2013 C-Max Hybrid VIN 5138

https://www.energy.gov/sites/prod/files/2015/02/f19/batteryC-Max5138.pdf

Confirms the same pack specification and provides measured laboratory power capability. Use the actual report tables when setting charge/discharge design limits rather than relying on remembered rounded values.

The current project checkpoint carries roughly **56 kW 10-second discharge / 42 kW 10-second charge at 50% DOD** as the representative tested capability from the DOE work; re-check the exact test condition and pack health before using it as a design limit.

### Ford C-Max owner documentation — battery cooling / service disconnect

2013 C-Max Hybrid owner guide:

https://www.fordservicecontent.com/Ford_Content/catalog/owner_guides/13cmhom2e.pdf

Useful confirmation that the C-Max high-voltage battery is air cooled and uses cabin-air openings around the rear package area. The project should retain Ford's cooling system initially, then only repackage the blower/ducting after temperature and fan-demand data justify it.

Ford service-disconnect information is also useful for understanding the OEM safety architecture. Always use the applicable factory service information for the exact donor year before handling the pack.

## Ford BECM / CAN behavior

### Ford OBD System Operation — Hybrid Electric Vehicle

https://www.fordservicecontent.com/ford_content/catalog/motorcraft/OBDSM1901_HEV.pdf

Useful BECM description:

- BECM manages high-voltage battery charging/discharging condition;
- monitors individual cell voltages and internal temperature sensors;
- monitors pack current;
- determines ability to receive/provide power;
- controls the battery cooling fan;
- controls high-voltage-battery-junction-box contactors;
- communicates with other modules over HS-CAN;
- documents loss-of-communication diagnostics such as U0293 with the Hybrid/EV Powertrain Control Module.

This supports the planned strategy of **retaining the Ford BECM and learning the minimum CAN message contract**, rather than replacing the OEM battery-management functions immediately.

### Earlier Ford HEV OBD system operation / block diagram

https://www.fordservicecontent.com/ford_content/catalog/motorcraft/OBDSM1503_HEV.pdf

Useful for the high-level BECM architecture: cell-voltage sensing, temperature sensing, current sensing, contactor/leakage sensing, service-disconnect/interlock inputs, contactor coils, cooling fan, and network relationships.

## Ford C-Max community CAN work

Useful public research repository identified during concept work:

https://github.com/cr08/Ford-C-Max-NA-Hybrid-PHEV-CAN-bus-research

Use as a reference for existing signal identification and DBC work. Do not assume it already contains a turnkey standalone BECM wake/READY sequence; validate against an intact donor or known-good logs.

## Evidence / design rules

- **FACTORY/DOE data outranks forum data.**
- OpenInverter/community material is valuable for conversion precedent and internal details but must be verified on the acquired hardware.
- Do not freeze battery dimensions from marketplace photos or tile-scale estimates; measure an actual complete Hybrid pack.
- Do not freeze the Q211 10,000 rpm design limit until supported by better source material or controlled testing.
- Do not assume every 2013–2018 C-Max Hybrid pack revision is electrically/mechanically identical.
- Do not assume Matrix/Q211 CV spline compatibility from part-number or boot-kit overlap; physically test it.
- Do not assume the ~4.07 custom gear pair is manufacturable as sketched until the OEM helical geometry is measured and a gear designer reviews it.

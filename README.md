# DRV8833 Dual H-Bridge Motor Driver PCB

A compact **2-layer PCB design for the Texas Instruments DRV8833 dual H-bridge motor driver**, developed in KiCad with emphasis on datasheet-driven schematic design, component placement, power decoupling, motor-current routing, PowerPAD thermal management, verification, and manufacturing-output preparation.

> **Project status:** Design complete • ERC = 0 • DRC = 0 • Final footprint corrections completed • Ready for final fabrication-package release

---

## Table of Contents

- [1. Project Overview](#1-project-overview)
- [2. Objectives](#2-objectives)
- [3. Key Features](#3-key-features)
- [4. DRV8833 Overview](#4-drv8833-overview)
- [5. Functional Architecture](#5-functional-architecture)
- [6. Schematic Design](#6-schematic-design)
- [7. Power and Decoupling Network](#7-power-and-decoupling-network)
- [8. Interface Connectors](#8-interface-connectors)
- [9. PCB Placement Strategy](#9-pcb-placement-strategy)
- [10. PCB Routing Strategy](#10-pcb-routing-strategy)
- [11. Grounding Strategy](#11-grounding-strategy)
- [12. PowerPAD and Thermal Design](#12-powerpad-and-thermal-design)
- [13. Component Footprint Selection](#13-component-footprint-selection)
- [14. LED Power Indicator](#14-led-power-indicator)
- [15. Design Iterations and Corrections](#15-design-iterations-and-corrections)
- [16. Verification and Validation](#16-verification-and-validation)
- [17. Gerber and Manufacturing Verification](#17-gerber-and-manufacturing-verification)
- [18. Final Design Verification Matrix](#18-final-design-verification-matrix)
- [19. Hardware Bring-Up Plan](#19-hardware-bring-up-plan)
- [20. Design Limitations](#20-design-limitations)
- [21. Engineering Lessons Learned](#21-engineering-lessons-learned)
- [22. Project Skills Demonstrated](#22-project-skills-demonstrated)
- [23. Repository Structure](#23-repository-structure)
- [24. Future Improvements](#24-future-improvements)
- [25. Interview Defence](#25-interview-defence)
- [26. Resume Description](#26-resume-description)
- [27. References](#27-references)

---

# 1. Project Overview

This project implements a compact motor-driver PCB based on the **TI DRV8833 dual H-bridge motor driver**.

The PCB provides:

- Two H-bridge motor channels
- Two motor-output pairs
- Four external logic-control inputs
- External SLEEP control
- External FAULT status
- Motor supply input
- Ground connection
- Local VM/VCP/VINT bypass capacitors
- Power-indicator LED
- Exposed PowerPAD thermal connection
- Thermal-via array
- Clearly labelled connectors

The project was designed as a practical **embedded-hardware / PCB-layout project**, with particular emphasis on translating IC datasheet requirements into an actual manufacturable PCB.

---

# 2. Objectives

The main objectives were:

1. Study the DRV8833 datasheet before PCB implementation.
2. Design the complete motor-driver schematic.
3. Select the correct DRV8833 PWP package and PCB footprint.
4. Implement the required supply and charge-pump capacitors.
5. Design a compact 2-layer PCB.
6. Place critical bypass components close to the IC.
7. Route motor-output and control signals appropriately.
8. Implement the exposed PowerPAD thermal path.
9. Provide thermal vias beneath the PowerPAD.
10. Provide practical external connectors.
11. Verify schematic connectivity using ERC.
12. Verify PCB geometry and connectivity using DRC.
13. Inspect the actual Gerber manufacturing artwork.
14. Correct design issues found during iterative review.
15. Prepare a final fabrication-ready project package.

---

# 3. Key Features

| Feature | Implementation |
|---|---|
| Motor driver | TI DRV8833 |
| Package | PWP / HTSSOP with exposed PowerPAD |
| PCB | 2-layer |
| Motor channels | 2 × H-bridge |
| Motor outputs | AOUT1/AOUT2 and BOUT1/BOUT2 |
| Logic inputs | AIN1/AIN2 and BIN1/BIN2 |
| Control | SLEEP |
| Status | FAULT |
| Motor supply | VM |
| VM bypass | 10 µF + 100 nF |
| Charge-pump bypass | 10 nF VCP-to-VM |
| Internal regulator bypass | 2.2 µF VINT-to-GND |
| Current-sense configuration | AISEN/BISEN tied to GND |
| Thermal management | PowerPAD + thermal vias |
| PCB verification | ERC = 0, DRC = 0 |
| Manufacturing outputs | Gerber + drill files |

---

# 4. DRV8833 Overview

The DRV8833 is a dual H-bridge motor driver intended for applications such as:

- Two brushed DC motors
- One bipolar stepper motor

The device integrates two NMOS H-bridge power stages along with control, protection and current-regulation functionality.

The project uses the following major DRV8833 functions:

### Motor A

- AIN1
- AIN2
- AOUT1
- AOUT2
- AISEN

### Motor B

- BIN1
- BIN2
- BOUT1
- BOUT2
- BISEN

### Power / internal supply

- VM
- VINT
- VCP
- GND
- PowerPAD

### Control / status

- SLEEP
- FAULT

The design uses the DRV8833 as the central motor-driver IC and exposes the required control and motor interfaces through two 1×6 connectors.

---

# 5. Functional Architecture

```text
                         DRV8833
                    ┌────────────────┐
                    │                │
        AIN1 ──────►│                │──────► AOUT1
        AIN2 ──────►│   H-BRIDGE A   │──────► AOUT2
                    │                │
        BIN1 ──────►│                │──────► BOUT1
        BIN2 ──────►│   H-BRIDGE B   │──────► BOUT2
                    │                │
        SLEEP ─────►│                │
        FAULT ◄─────│                │
                    │                │
        VM ────────►│ Motor Supply   │
        VCP ───────►│ Charge Pump    │
        VINT ──────►│ Internal Supply│
        GND ───────►│                │
                    │   PowerPAD     │
                    └───────┬────────┘
                            │
                       Thermal Vias
                            │
                          GND
```

---

# 6. Schematic Design

The schematic was developed around the DRV8833 functional requirements and the recommended power network.

The design includes:

- DRV8833PWP
- C1 = 10 nF
- C2 = 10 µF
- C3 = 100 nF
- C4 = 2.2 µF
- D1 = LED
- R1 = 4.7 kΩ
- J1 = 1×6 connector
- J2 = 1×6 connector

The final schematic was reviewed for:

- Correct pin mapping
- Power connections
- Capacitor connections
- Motor outputs
- Logic inputs
- SLEEP
- FAULT
- Current-sense configuration
- LED polarity
- Connector mapping

---

# 7. Power and Decoupling Network

Power integrity was one of the most important design aspects.

## 7.1 C1 — VCP Charge-Pump Capacitor

```text
VCP ───── C1 10 nF ───── VM
```

C1 is the charge-pump capacitor. It was deliberately placed close to the DRV8833 VCP/VM region to minimize loop area and parasitic impedance.

**Final requirement:**

- Value: 10 nF
- Connection: VCP ↔ VM
- Type: Ceramic MLCC
- Placement: Close to U1

## 7.2 C2 — Main VM Bypass

```text
VM ───── C2 10 µF ───── GND
```

C2 provides the primary local motor-supply bypass and is located close to the DRV8833 supply path.

## 7.3 C3 — High-Frequency VM Bypass

```text
VM ───── C3 100 nF ───── GND
```

C3 provides additional local high-frequency bypassing.

The C3 footprint was reviewed during the design iterations and changed to an appropriate ceramic-MLCC footprint so that the schematic component, footprint and physical component remain consistent.

> **Important lesson:** A PCB can pass ERC/DRC while still having an inappropriate component footprint. Footprint and BOM verification are therefore separate engineering checks.

## 7.4 C4 — VINT Bypass

```text
VINT ───── C4 2.2 µF ───── GND
```

C4 provides the local bypass for the internal VINT regulator and was placed close to the corresponding DRV8833 pins.

---

# 8. Interface Connectors

## J1 — Output / Control-Status Connector

| Pin | Signal |
|---:|---|
| 1 | SLEEP |
| 2 | AOUT1 |
| 3 | AOUT2 |
| 4 | BOUT1 |
| 5 | BOUT2 |
| 6 | FAULT |

## J2 — Logic / Power Connector

| Pin | Signal |
|---:|---|
| 1 | AIN1 |
| 2 | AIN2 |
| 3 | VM |
| 4 | GND |
| 5 | BIN1 |
| 6 | BIN2 |

This organization keeps the interface easy to understand during external-system integration and debugging.

---

# 9. PCB Placement Strategy

The placement process was based on electrical function rather than simply minimizing ratsnest length.

## 9.1 DRV8833 Placement

U1 was placed approximately at the center of the PCB. This provides short and practical connections toward:

- Motor outputs
- Logic inputs
- VM supply
- Local bypass capacitors
- Ground
- PowerPAD thermal structure

## 9.2 Capacitor Placement

The critical capacitors were kept near their associated IC pins:

```text
VCP ─ C1 ─ VM
VINT ─ C4 ─ GND
VM ─ C2 ─ GND
VM ─ C3 ─ GND
```

This minimizes parasitic loop area.

## 9.3 Connector Placement

The two connectors were positioned at opposite sides of the board to improve accessibility and simplify external wiring.

---

# 10. PCB Routing Strategy

The routing was divided conceptually into:

### Power / motor-current routing

- VM
- AOUT1
- AOUT2
- BOUT1
- BOUT2
- GND

### Small-signal routing

- AIN1
- AIN2
- BIN1
- BIN2
- SLEEP
- FAULT

Motor-output routes were given wider routing than the small-signal control routes where practical.

The final motor-output routing uses approximately **0.3 mm routing** in the relevant paths.

> The 0.3-mm trace width demonstrates current-aware routing for this compact prototype. It should not be interpreted as guaranteeing the DRV8833's maximum datasheet current under all operating conditions.

---

# 11. Grounding Strategy

Ground is used as:

- IC power return
- Bypass-capacitor return
- Motor-driver return
- PowerPAD thermal return
- Logic reference

The design uses copper and vias to provide low-impedance ground paths.

The exposed PowerPAD is connected to the ground structure and thermal vias provide a path toward the opposite copper layer.

---

# 12. PowerPAD and Thermal Design

The DRV8833 PWP package contains an exposed PowerPAD. This pad is important for thermal management.

The PCB implementation follows the general concept:

```text
                DRV8833
             ┌───────────┐
             │ PowerPAD  │
             └─────┬─────┘
                   │
              GND Copper
                   │
            Thermal Via Array
                   │
             Bottom Copper
                   │
                  GND
```

The thermal-via array:

- Transfers heat away from the package
- Connects the exposed pad to bottom copper
- Provides an electrical GND connection
- Increases effective thermal copper area

This is one of the major datasheet-driven aspects of the PCB.

---

# 13. Component Footprint Selection

Footprint selection was treated as an engineering task rather than a purely mechanical operation.

The final footprint review included:

- U1 package
- C1 footprint
- C2 footprint
- C3 footprint
- C4 footprint
- D1 footprint
- R1 footprint
- Connector footprints

A particularly important correction involved C3.

### C3 initial problem

The schematic value was:

```text
100 nF
```

but the assigned footprint was not appropriate for the intended ceramic MLCC.

### Final correction

C3 was changed to a suitable ceramic-MLCC footprint.

This ensured consistency between:

```text
Schematic
   ↓
Value
   ↓
Footprint
   ↓
BOM
   ↓
Physical component
```

---

# 14. LED Power Indicator

The PCB includes a VM power indicator:

```text
VM
 │
 D1 LED
 │
R1 4.7 kΩ
 │
GND
```

D1 polarity was reviewed and corrected during the iterative design process.

The LED provides a simple visual indication that the motor-driver supply is present.

---

# 15. Design Iterations and Corrections

The PCB was developed through multiple review iterations.

| Issue | Correction |
|---|---|
| VCC naming | Renamed and propagated to VM |
| AOUT2 routing | Corrected to continuous routing |
| D1 polarity | Corrected |
| C1 network | Verified as VCP ↔ VM |
| C2 network | Verified as VM ↔ GND |
| C4 network | Verified as VINT ↔ GND |
| Capacitor placement | Reviewed for short local loops |
| Motor routing | Improved using wider output traces |
| Duplicate BOUT2 via | Removed |
| C3 footprint | Corrected to suitable ceramic-MLCC footprint |
| PowerPAD thermal structure | Reviewed and retained |
| Gerber outputs | Regenerated/reviewed after corrections |

This iterative process was an important part of the project.

---

# 16. Verification and Validation

The design was verified at multiple levels.

## 16.1 Schematic Verification

Checked:

- Pin connectivity
- Power network
- Capacitor connections
- Connector mappings
- Motor outputs
- Logic inputs
- SLEEP
- FAULT
- AISEN/BISEN
- LED polarity

## 16.2 ERC

Final reported result:

```text
ERC = 0 errors
```

## 16.3 PCB DRC

Final reported result:

```text
DRC = 0 errors
```

## 16.4 Connectivity Review

Important final nets were reviewed individually:

```text
AIN1  AIN2  BIN1  BIN2
AOUT1 AOUT2 BOUT1 BOUT2
VM    GND   VCP   VINT
SLEEP FAULT AISEN BISEN
```

---

# 17. Gerber and Manufacturing Verification

The manufacturing artwork was reviewed after PCB routing.

The final release is intended to contain:

```text
F.Cu
B.Cu
F.Mask
B.Mask
F.Paste
B.Paste
F.Silkscreen
B.Silkscreen
Edge.Cuts
PTH Drill
NPTH Drill
```

The Gerber review checked:

- Copper continuity
- Pad geometry
- Via placement
- Motor-output routing
- VM naming
- Mask openings
- Paste features
- Silkscreen
- Board outline
- Thermal structure

> **Important:** All fabrication outputs should be regenerated from the same final PCB revision. Do not mix Gerbers or drill files from earlier revisions.

---

# 18. Final Design Verification Matrix

| Item | Final Status |
|---|---|
| DRV8833 package | ✅ Verified |
| Schematic connectivity | ✅ Verified |
| VM naming | ✅ Correct |
| C1 VCP ↔ VM | ✅ Correct |
| C2 VM ↔ GND | ✅ Correct |
| C3 VM ↔ GND | ✅ Correct |
| C4 VINT ↔ GND | ✅ Correct |
| AISEN → GND | ✅ Correct |
| BISEN → GND | ✅ Correct |
| AOUT1 routing | ✅ Verified |
| AOUT2 routing | ✅ Corrected and verified |
| BOUT1 routing | ✅ Verified |
| BOUT2 routing | ✅ Verified |
| Duplicate BOUT2 via | ✅ Removed |
| D1 polarity | ✅ Corrected |
| C3 footprint | ✅ Corrected |
| PowerPAD | ✅ Connected to GND |
| Thermal vias | ✅ Implemented |
| ERC | ✅ 0 |
| DRC | ✅ 0 |
| Gerber layers | ✅ Generated/reviewed |
| Board outline | ✅ Verified |

---

# 19. Hardware Bring-Up Plan

After fabrication, the recommended validation sequence is:

### Step 1 — Visual Inspection

Check:

- U1 orientation
- Component placement
- Capacitor footprints
- LED polarity
- Connector orientation
- Solder bridges
- PowerPAD soldering
- Thermal vias

### Step 2 — Unpowered Continuity

Check the VM-to-GND resistance/continuity for unintended shorts and verify important net continuity.

### Step 3 — Current-Limited Power-Up

Use a bench supply with an appropriate VM voltage and conservative current limit.

### Step 4 — Verify VM

Measure VM directly on the PCB.

### Step 5 — Verify Logic Inputs

Test AIN1, AIN2, BIN1 and BIN2.

### Step 6 — Verify SLEEP

Test enabling/disabling the driver.

### Step 7 — Test Motor A

Test forward, reverse and sleep/coast/brake behavior as applicable.

### Step 8 — Test Motor B

Repeat for the second H-bridge.

### Step 9 — Thermal Test

Measure IC temperature, motor current and PCB temperature under the intended load.

---

# 20. Design Limitations

### 20.1 Current Capability

The DRV8833 has a specified output-current capability, but the finished PCB should not automatically be considered capable of the maximum IC current continuously.

Actual performance depends on:

- Motor current
- Stall current
- Duty cycle
- Ambient temperature
- PCB copper
- Package temperature
- Thermal resistance
- Operating time

### 20.2 Current Sense

AISEN and BISEN are tied to GND. Therefore this design does not implement external current-sense resistors.

### 20.3 nFAULT Pull-Up

The FAULT output is exposed to the external controller. Because nFAULT is an open-drain output, the external host should provide an appropriate pull-up if one is not provided elsewhere in the system.

### 20.4 Thermal Characterization

The PCB includes the intended PowerPAD and thermal-via implementation, but actual thermal performance should be experimentally characterized on fabricated hardware.

---

# 21. Engineering Lessons Learned

## Datasheet Study Is Part of PCB Design

The datasheet directly influenced component values, component placement, power routing, thermal structure, package footprint and solder-paste considerations.

## Placement Is More Important Than Just Ratsnest Optimization

A component can be electrically connected while still being poorly placed. For this project, C1, C2, C4 and U1 placement were particularly important.

## ERC/DRC Are Necessary but Not Sufficient

A design can have ERC = 0 and DRC = 0 and still have a wrong footprint, wrong component, incorrect datasheet implementation or poor thermal design.

## Gerber Review Matters

The Gerber is the actual manufacturing representation, so final PCB verification should not stop at the KiCad PCB editor.

## Thermal Design Is Functional Design

For a PowerPAD motor driver, thermal vias and copper area directly influence device temperature.

## Motor Routing Requires Current Awareness

Motor outputs should not automatically be routed exactly like low-current logic signals.

## Footprint Verification Is Essential

The physical footprint must match the actual component. The C3 correction demonstrated this clearly.

---

# 22. Project Skills Demonstrated

### PCB Design

- Schematic capture
- PCB placement
- Two-layer routing
- Via usage
- Copper management
- Silkscreen design
- Board-outline definition

### Embedded Hardware

- Motor-driver integration
- Logic interfaces
- Power interfaces
- External controller interfaces
- Motor-output interfaces

### Datasheet-Based Design

- Pin-function interpretation
- Power requirements
- Bypass requirements
- Package requirements
- Thermal requirements
- Layout recommendations

### Verification

- ERC
- DRC
- Net-level inspection
- Gerber inspection
- Footprint verification

### Manufacturing

- Gerber generation
- Drill-file generation
- Solder-mask review
- Paste review
- Silkscreen review
- Edge-Cuts verification

---

# 23. Repository Structure

Recommended GitHub repository structure:

```text
DRV8833-PCB/
│
├── README.md
│
├── Documentation/
│   ├── DRV8833_PCB_Design_Report.pdf
│   ├── DRV8833_PCB_Design_Report.docx
│   ├── Design_Notes.md
│   └── Verification_Report.md
│
├── Schematic/
│   └── Motor_Driver_DRV8833.kicad_sch
│
├── PCB/
│   └── Motor_Driver_DRV8833.kicad_pcb
│
├── Project/
│   └── Motor_Driver_DRV8833.kicad_pro
│
├── Manufacturing/
│   ├── Gerbers/
│   │   ├── F_Cu.gbr
│   │   ├── B_Cu.gbr
│   │   ├── F_Mask.gbr
│   │   ├── B_Mask.gbr
│   │   ├── F_Paste.gbr
│   │   ├── B_Paste.gbr
│   │   ├── F_Silkscreen.gbr
│   │   ├── B_Silkscreen.gbr
│   │   └── Edge_Cuts.gbr
│   │
│   └── Drill/
│       ├── PTH.drl
│       └── NPTH.drl
│
├── BOM/
│   └── BOM.csv
│
├── Verification/
│   ├── ERC_Report/
│   └── DRC_Report/
│
└── Images/
    ├── Schematic.png
    ├── PCB_3D.png
    ├── PCB_Front.png
    └── PCB_Back.png
```

---

# 24. Future Improvements

Possible future revisions include:

- Add explicit external current-sense resistors.
- Add selectable/configurable nFAULT pull-up.
- Add additional power-entry protection if required by the application.
- Add reverse-polarity protection.
- Add input transient/ESD protection.
- Increase copper/trace width for higher-current applications where justified by calculations.
- Perform thermal measurements on the fabricated PCB.
- Perform motor-load and stall-current testing.
- Add mounting holes if mechanical integration requires them.
- Add test points for VM, GND, AIN1/AIN2, BIN1/BIN2 and FAULT.
- Add a dedicated programming/control header if required by the host system.

These are **future enhancements**, not required changes to the completed project.

---

# 25. Interview Defence

### 30-second project explanation

> I designed a compact two-layer PCB around the TI DRV8833 dual H-bridge motor driver using KiCad. I started by studying the datasheet, particularly the VM, VCP and VINT supply requirements and the PWP PowerPAD thermal recommendations. I implemented the 10-µF VM bypass, 10-nF VCP-to-VM capacitor and 2.2-µF VINT bypass close to the IC, routed the motor outputs with wider traces, and implemented a PowerPAD thermal-via structure. During iterative review I corrected AOUT2 routing, LED polarity, VM naming, a duplicate BOUT2 via and the C3 footprint. The final design was verified with ERC = 0 and DRC = 0, followed by Gerber-level review.

### Why was the PowerPAD important?

The PWP package exposes the thermal pad underneath the IC. It provides an important thermal path into the PCB. The pad was connected to GND copper and supported with thermal vias toward the opposite copper layer.

### Why is C1 connected between VCP and VM?

C1 is the DRV8833 charge-pump capacitor. It is not a normal VCP-to-GND bypass capacitor.

```text
VCP ── 10 nF ── VM
```

### Why are C2 and C3 connected to VM?

They provide local VM bypassing:

```text
VM ── 10 µF ── GND
VM ── 100 nF ── GND
```

### Why is C4 connected to VINT?

C4 provides the local bypass for the DRV8833 internal VINT regulator:

```text
VINT ── 2.2 µF ── GND
```

### Why were motor traces wider?

The motor outputs carry significantly more current than the logic inputs, so the output routing was given wider copper where practical.

### Why wasn't DRC enough?

DRC verifies PCB rules such as clearance and geometry. It does not prove that the selected physical component footprint is correct for the intended component. The C3 footprint review demonstrated this.

### What was the most important layout consideration?

The most important considerations were the local power/charge-pump capacitor placement and the PowerPAD thermal implementation. Motor-output routing was also handled with current in mind.

---

# 26. Resume Description

### DRV8833 Dual H-Bridge Motor Driver PCB

> Designed and laid out a compact **2-layer DRV8833 motor-driver PCB in KiCad**, integrating dual H-bridge motor outputs, logic/control interfaces, VM/VCP/VINT decoupling and connectorized system interfaces.

> Implemented **datasheet-aligned power and thermal layout**, including local ceramic bypassing, PowerPAD-to-GND thermal vias, motor-current routing, footprint verification and iterative **ERC/DRC and Gerber-level validation**.

---

# 27. References

### Primary reference

**Texas Instruments — DRV8833 Dual H-Bridge Motor Driver Datasheet**

The datasheet was used as the primary design reference for:

- Pin configuration
- Power-supply requirements
- Capacitor selection
- VCP charge-pump implementation
- VINT bypassing
- VM bypassing
- Thermal layout
- PowerPAD
- Package dimensions
- Recommended land pattern
- Solder-paste considerations

### Project tool

**KiCad**

Used for:

- Schematic capture
- PCB layout
- Footprint assignment
- Routing
- ERC
- DRC
- Gerber generation

---

# Final Project Status

## ✅ DESIGN COMPLETE

The DRV8833 PCB has completed the major design and verification stages:

```text
Datasheet Study                 ✅
Schematic Design                ✅
Component Selection             ✅
Footprint Assignment            ✅
PCB Placement                   ✅
Power Routing                   ✅
Motor Routing                   ✅
Logic Routing                   ✅
Ground / Thermal Design         ✅
PowerPAD Implementation         ✅
Footprint Review                ✅
ERC                             ✅ 0
DRC                             ✅ 0
Gerber Generation               ✅
Gerber Review                   ✅
Final Design Corrections        ✅
```

### Final assessment

This project demonstrates **basic-to-moderate practical PCB layout and embedded-hardware design capability**, with particular strength in **datasheet-driven design, power/decoupling implementation, motor-driver routing, thermal management, footprint verification, iterative debugging and manufacturing verification**.

It should be presented as a **practical embedded PCB design project**, rather than as an advanced high-current power-electronics design.

---

**Designed in KiCad | DRV8833 Motor Driver | 2-Layer PCB | ERC = 0 | DRC = 0**

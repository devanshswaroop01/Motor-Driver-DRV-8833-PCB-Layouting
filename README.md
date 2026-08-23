# DRV8833 Dual H-Bridge Motor Driver PCB

A 2-layer KiCad PCB for the Texas Instruments DRV8833 dual H-bridge motor driver, with datasheet-driven schematic design, power decoupling, PowerPAD thermal management, and full manufacturing-output verification.

**Status:** Design complete · ERC = 0 · DRC = 0 · Ready for fabrication

---

## Overview

This PCB provides two H-bridge motor channels, four logic control inputs, SLEEP/FAULT control lines, a motor supply input, local VM/VCP/VINT bypass capacitors, a power-indicator LED, and an exposed PowerPAD thermal connection with a thermal-via array.

## Features

| Feature | Implementation |
|---|---|
| Motor driver | TI DRV8833, PWP (HTSSOP) package with exposed PowerPAD |
| PCB | 2-layer |
| Motor channels | 2 × H-bridge (AOUT1/AOUT2, BOUT1/BOUT2) |
| Logic inputs | AIN1/AIN2, BIN1/BIN2 |
| Control / status | SLEEP, FAULT |
| VM bypass | 10 µF + 100 nF |
| Charge-pump cap | 10 nF, VCP–VM |
| VINT bypass | 2.2 µF, VINT–GND |
| Current sense | AISEN/BISEN tied to GND (no external sense resistors) |
| Thermal | PowerPAD + thermal via array |
| Verification | ERC = 0, DRC = 0, Gerber-level review |

## Schematic Design

| Ref | Value | Function |
|---|---|---|
| U1 | DRV8833PWP | Dual H-bridge driver |
| C1 | 10 nF | VCP–VM charge-pump cap |
| C2 | 10 µF | VM–GND bulk bypass |
| C3 | 100 nF | VM–GND high-freq bypass |
| C4 | 2.2 µF | VINT–GND bypass |
| D1 | LED | Power indicator |
| R1 | 4.7 kΩ | LED current limit |
| J1 | 1×6 | SLEEP, AOUT1/2, BOUT1/2, FAULT |
| J2 | 1×6 | AIN1/2, VM, GND, BIN1/2 |

## Layout Notes

- U1 placed centrally for short, direct routing to motor outputs, logic inputs, and bypass caps.
- C1, C2, C4 placed close to their respective IC pins to minimize loop area.
- Motor-output traces (~0.3 mm) routed wider than logic-signal traces to account for higher current. This is appropriate for the compact prototype scale of this board and is not a claim of continuous full-datasheet current capability.
- Exposed PowerPAD tied to GND copper and connected to bottom copper through a thermal via array.

## Verification

- **ERC:** 0 errors — pin connectivity, power network, connector mapping, and LED polarity checked.
- **DRC:** 0 errors — geometry and clearance checked.
- **Gerber review:** copper continuity, pad geometry, via placement, and silkscreen checked against the final PCB revision (F.Cu, B.Cu, masks, paste, silkscreen, Edge.Cuts, PTH/NPTH drill).

Note: ERC/DRC passing does not guarantee correct component footprints — a footprint mismatch on C3 (wrong ceramic MLCC land pattern) was caught only during manual footprint review and corrected.

## Design Iterations

| Issue | Correction |
|---|---|
| VCC naming inconsistency | Renamed to VM throughout |
| AOUT2 routing | Fixed broken/discontinuous route |
| D1 polarity | Corrected |
| Duplicate BOUT2 via | Removed |
| C3 footprint | Corrected to proper ceramic-MLCC land pattern |

## Known Limitations

- No external current-sense resistors (AISEN/BISEN grounded).
- nFAULT is open-drain — host system must provide its own pull-up if not present elsewhere.
- Actual current handling and thermal performance depend on ambient conditions, duty cycle, and copper area, and have not yet been measured on fabricated hardware.

## Bring-Up Checklist

1. Visual inspection (orientation, solder bridges, PowerPAD soldering, thermal vias).
2. Unpowered continuity check (VM–GND, key nets).
3. Power up on a bench supply with a conservative current limit.
4. Verify VM at the board.
5. Verify logic inputs (AIN1/2, BIN1/2).
6. Verify SLEEP enable/disable.
7. Test Motor A (forward/reverse/coast/brake).
8. Test Motor B.
9. Thermal test under expected load.

## Repository Structure
DRV8833-PCB/
├── README.md

├── Documentation/

├── Schematic/Motor_Driver_DRV8833.kicad_sch

├── PCB/Motor_Driver_DRV8833.kicad_pcb

├── Project/Motor_Driver_DRV8833.kicad_pro

├── Manufacturing/

│ ├── Gerbers/

│ └── Drill/

├── BOM/BOM.csv

├── Verification/

│ ├── ERC_Report/

│ └── DRC_Report/

└── Images/


## Future Work

- External current-sense resistors
- nFAULT pull-up option
- Reverse-polarity and ESD protection
- Test points for VM, GND, AIN1/2, BIN1/2, FAULT
- Measured thermal and stall-current data on fabricated hardware

## Tools & References

- **KiCad** — schematic capture, PCB layout, ERC, DRC, Gerber generation
- **TI DRV8833 Datasheet** — primary reference for pinout, power/thermal requirements, and PowerPAD land pattern

---

*2-layer KiCad PCB · DRV8833 dual H-bridge motor driver · ERC = 0 · DRC = 0*


# USB-C Sensor Interface PCB — RP2040 + Si7021

**Status:** In progress. Schematic and PCB layout are being designed in KiCad. Screenshots, ERC/DRC results, manufacturing export, assembly, and bench testing are not yet documented here.

[![Schematic](media/schematic.png)](media/schematic.png)

[![PCB layout](media/pcb.png)](media/pcb.png)

[![PCB 3D render](media/3d.png)](media/3d.png)

*(Images pending — the `media/` folder is in place but screenshots have not been added yet.)*

## What This Is

A custom RP2040-based PCB with native USB-C connectivity for programming and power, integrating a Si7021 temperature and humidity sensor over I²C. The board applies USB 2.0 signal-integrity techniques — matched-length, controlled-impedance differential pairs on the USB data lines — and builds on the earlier [Blinking-LED-Documentation](https://github.com/nizarbadriali/Blinking-LED-Documentation) and [LED-PCB-Documentation](https://github.com/nizarbadriali/LED-PCB-Documentation) projects.

## How the Circuit Works

| Block | Parts | Role |
| ----- | ----- | ---- |
| Microcontroller | RP2040 | Dual-core MCU; runs firmware and provides native USB 1.1 device (Full Speed, USB 2.0-compatible) on its D+/D− pins. |
| Program storage | QSPI flash | External flash the RP2040 boots from. Part number and capacity pending. |
| Clock | Crystal oscillator | 12 MHz reference for the RP2040 PLLs (confirm value and load capacitors against the RP2040 hardware design guide). |
| USB-C connector | USB-C receptacle, CC1/CC2 pull-downs | 5.1 kΩ pull-downs on CC1 and CC2 identify the board as a USB sink so a USB-C source supplies VBUS. |
| USB data | D+ / D− differential pair | Routed as a matched-length, controlled-impedance pair (90 Ω differential target) with minimal vias. Series resistors per the RP2040 guide pending confirmation. |
| Power | Regulator, decoupling | VBUS (5 V) down to 3.3 V for the RP2040 IO, flash, and sensor. Regulator part number pending. |
| Sensor | Si7021 | Temperature and relative-humidity sensor on the RP2040 I²C bus. Pull-up values pending. |

## Results

| Parameter | Value | Evidence or qualification |
| --------- | ----- | ------------------------- |
| USB D+/D− differential impedance | pending | Target 90 Ω; depends on stackup, trace width and spacing — record the fab's stackup and calculator result |
| D+/D− length mismatch | pending | Report from KiCad length-tuning tool not yet uploaded |
| USB enumeration | pending | Not yet assembled or tested |
| Si7021 readings | pending | Requires firmware and assembled board |
| ERC / DRC / parity | pending | Results not yet uploaded |
| Manufacturing package | pending | Gerbers and drills not yet uploaded |

## Quick Start

**View the design:** Open the `.kicad_pro` file in the `kicad/` folder in KiCad, then open the schematic and PCB from the project manager.

**Run the simulation / analysis scripts:** *(pending)* Add scripts to `simulation/python/` or `simulation/matlab/` and describe how to run them here.

## Bill of Materials

| Reference | Value | Package / specification | Selection status |
| --------- | ----- | ----------------------- | ---------------- |
| U1 | RP2040 | QFN-56 (7 × 7 mm) | Confirmed by part family; decoupling and layout per RP2040 hardware design guide |
| U2 | QSPI flash | Package pending | Part number and capacity pending |
| U3 | Si7021 | DFN-6 (3 × 3 mm) | Confirm I²C address and supply range in the datasheet |
| U4 | 3.3 V regulator | Package pending | Part number, dropout, and current rating pending |
| Y1 | 12 MHz crystal | Package pending | Load capacitance and part number pending |
| J1 | USB-C receptacle | Footprint pending | Manufacturer part number pending |
| R (CC1, CC2) | 5.1 kΩ | Package pending | Tolerance and part number pending |
| C (decoupling) | pending | Package pending | Values per RP2040 guide pending |

## Project Files

| Path | Contents |
| ---- | -------- |
| `kicad/` | Project, schematic, PCB, and manufacturing ZIP (to be added) |
| `media/` | Schematic, PCB, and 3D render screenshots (to be added) |
| `simulation/` | Simulation and analysis scripts (`matlab/`, `python/`) |
| `models/` | Component models (e.g. SPICE / IBIS) |
| `workdir/` | Supporting data |
| `DESIGN_NOTES.md` | Design decisions, stackup, revisions, checks, and fabrication details (to be written) |

## Limitations and Next Steps

- Upload schematic, PCB, and 3D screenshots to `media/`.
- Record the PCB stackup and the trace width/spacing used to hit the USB differential impedance target.
- Run ERC, DRC, and schematic-to-PCB parity checks and upload the results.
- Select real part numbers for flash, regulator, crystal, and USB-C connector and check datasheets against footprints.
- Assemble, confirm USB enumeration and Si7021 readings before claiming physical validation.

## Tools and Attribution

- KiCad: schematic and PCB design (record the exact version from the project files).
- Raspberry Pi *Hardware design with RP2040*: reference for flash, crystal, USB, and power circuitry.
- Si7021 datasheet: reference for I²C interface and supply requirements.

## License

MIT License. See [LICENSE](LICENSE).

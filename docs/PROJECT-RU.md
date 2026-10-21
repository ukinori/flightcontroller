# Hardware opening, inspection and bring-up guide

This guide is entirely in English. Its legacy filename is retained for existing links.

## Open the project

1. Download the whole repository and extract it into one directory.
2. In KiCad 10, open flightcontroller.kicad_pro.
3. Open flightcontroller.kicad_sch for the schematic and flightcontroller.kicad_pcb for the routed board.
4. Keep untitled.kicad_sch: it is the required MCU sheet.
5. Install the standard KiCad libraries. Project-specific libraries are bundled beside the project using ${KIPRJMOD} paths.

## Inspect the PCB

1. Review the main README for connector pinouts and output/GPIO/resistor mapping.
2. Enable one copper layer at a time in PCB Editor's Appearance panel.
3. F.Cu contains components, routing and GND/VCC_SERVO pours.
4. In1.Cu is the GND plane without ordinary routed tracks.
5. In2.Cu combines a 3.3 V pour and routing; it is not a continuous power plane.
6. B.Cu carries routing and +5V/VCC_SERVO pours.
7. Highlight a net and follow it through vias. A series resistor separates two differently named nets; highlight both sides separately.
8. Refill zones and run ERC/DRC after changes, reviewing ignored checks as well as reported findings.

A copper zone belongs to one net. Vias connect copper on different layers. Different nets need the required clearance.

## Connections

- J4: regulated 5 V, pin 1 positive and pin 2 GND. Never connect a raw 6–12S battery directly; use a separate external converter.
- J22: separate regulated 6 V servo supply, pin 1 VCC_SERVO and pin 2 GND.
- Outputs: pin 1 signal, pin 3 GND. Pin 2 is unconnected on OUT1–OUT6 and is VCC_SERVO on OUT7–OUT12.
- The 10 A servo-rail target is provisional, not a qualified rating. Validate actual stall current, BEC, wiring, connector, copper and heat rise.
- Intended allocation: DShot on OUT1–OUT5 and PWM on OUT7–OUT11, requiring custom PX4 timer/DMA integration.
- Controller TX connects to peripheral RX and controller RX to peripheral TX. Verify voltage levels and cable pinouts.

## Verification scope

The 2026-10-21 flight-controller publication snapshot reports zero ERC violations, physical DRC violations, unconnected items and schematic/PCB parity issues under the existing project settings. Reports are in verification/. Ignored checks are listed in the main README and JSON reports. Earlier U8/L1 parity warnings were not reproduced.

These results do not establish electrical operation, current rating, fabrication compliance or flight safety. The separate meshtransciever project is unfinished; its PCB is a placeholder and is outside this verification scope.

## Before fabrication and bring-up

1. Review the schematic, complete power budget, footprints, mechanics, sensor supplies, switching regulator and return paths.
2. Select a manufacturer and agree the four-layer stack-up, nominal 1.6 mm thickness, copper weights, 0.15 mm tracks/clearances and 0.20 mm via drills.
3. Select actual servos/BEC and validate the complete power path for combined peak/stall current.
4. Complete the purchasing BOM; existing CSV files cover assignments and selected parts.
5. Develop the dedicated PX4 port and bootloader. Firmware for another board does not establish compatibility.
6. First use a current-limited regulated 5 V bench supply without motors, servos or propellers. Check shorts, rails, SWD and oscillator operation before testing interfaces individually.
7. Test transients, sensors, outputs, heat and fault behaviour before aircraft use.

Editor history, temporary files and restore backups are excluded. Third-party library notices are retained.

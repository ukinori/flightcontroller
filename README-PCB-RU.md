# PCB routing and review notes

English edition; the legacy filename is retained for existing links. The routed flight-controller PCB is approximately 70 × 100 mm, with four copper layers and nominal 1.6 mm thickness.

## Implemented layout

Components are placed, connections routed and copper zones filled. In1.Cu is a GND plane without ordinary signal tracks. F.Cu contains component connections and GND/servo-power pours. In2.Cu carries a 3.3 V pour plus routing; B.Cu carries routing and +5V/VCC_SERVO pours.

C6/C7 were corrected from a series arrangement to separate parallel decoupling connections between +3.3V and GND. J22 uses Phoenix Contact 1715721 for the separate servo input.

## Verification history

The 2026-10-13 snapshot had zero reported physical DRC violations and unconnected items, with three CLI parity warnings involving L1/1, U8/7 and U8/8. An independent exported-netlist check matched 474 pin assignments with no missing pads.

The **2026-10-21 publication snapshot** reports **zero ERC violations, zero physical DRC violations, zero unconnected items and zero schematic/PCB parity issues**. The older warnings were not reproduced. Fresh reports are in verification/. Root DRC-final.json, ERC-final.json and netlist-verification.json are historical records.

Existing ignored check categories are listed in the main README and JSON reports. CAD checks do not establish function, current rating, fabrication suitability or flight readiness.

## Power and outputs

- J4: regulated 5 V controller input. A 6–12S battery requires a separate external converter.
- J22: separate regulated 6 V servo input, pin 1 VCC_SERVO and pin 2 GND. Grounds are common.
- Output headers: pin 1 signal, pin 3 GND. Pin 2 is unconnected on OUT1–OUT6 and is VCC_SERVO on OUT7–OUT12.
- Intended allocation: five DShot ESCs on OUT1–OUT5 and five PWM servos on OUT7–OUT11; OUT6/OUT12 spare. Custom PX4 timer/DMA configuration remains required.
- The servo rail has a provisional 10 A combined target, not a verified rating. Validate actual stall current, the BEC, wiring, connector, copper and temperature rise.

The main README contains the complete connector/GPIO/resistor map.

## Before fabrication and bring-up

1. Refill zones and rerun ERC/DRC after changes; review the ignored checks.
2. Agree the four-layer stack-up, copper weights, 0.20 mm via drills and 0.15 mm track/clearance capability with the manufacturer.
3. Review converter layout, power distribution, signal return paths, USB/SDMMC and thermal behaviour.
4. Check connector orientation, mounting clearance, sensor axes, barometer access and actual purchasing-part footprints. Complete the BOM.
5. Implement the custom PX4 port and bootloader, including clocks, sensors, buses, ADC, timers/DMA and sensor orientations.
6. Start from a current-limited regulated 5 V bench supply without ESCs, servos or propellers. Check shorts, rails, SWD and oscillator startup, then interfaces and loads.

This is a routed prototype, not a qualified production release or flight-tested firmware package.

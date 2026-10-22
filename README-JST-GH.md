# JST-GH interface revision

This is a custom STM32H743 flight controller, not a Pixhawk 6C hardware clone. Connector pin numbers refer to PCB pad numbers, not an assumed wire colour or a guessed mating-face view.

## Port wiring

### J1 — SWD_CUSTOM_GH5

Header: SM05B-GHS-TB. Cable housing: GHR-05V-S. Pitch: 1.25 mm. Contact: SSHL-002T-P0.2.

- Pin 1: /MCU/SWDIO
- Pin 2: /MCU/SWCLK
- Pin 3: NRST
- Pin 4: +3.3V
- Pin 5: GND

### J17 — RC_UART_GH6_NO_FLOW

Header: SM06B-GHS-TB. Cable housing: GHR-06V-S. Pitch: 1.25 mm. Contact: SSHL-002T-P0.2.

- Pin 1: +5V
- Pin 2: RC_TX
- Pin 3: RC_RX
- Pin 4: Not connected
- Pin 5: Not connected
- Pin 6: GND

### J18 — GPS_PIXHAWK_GPS2

Header: SM06B-GHS-TB. Cable housing: GHR-06V-S. Pitch: 1.25 mm. Contact: SSHL-002T-P0.2.

- Pin 1: +5V
- Pin 2: GPS_TX
- Pin 3: GPS_RX
- Pin 4: EXT_I2C_SCL
- Pin 5: EXT_I2C_SDA
- Pin 6: GND

### J19 — TELEM_GH6_NO_FLOW

Header: SM06B-GHS-TB. Cable housing: GHR-06V-S. Pitch: 1.25 mm. Contact: SSHL-002T-P0.2.

- Pin 1: +5V
- Pin 2: TELEM_TX
- Pin 3: TELEM_RX
- Pin 4: Not connected
- Pin 5: Not connected
- Pin 6: GND

### J20 — COMPASS_I2C4_5V

Header: SM04B-GHS-TB. Cable housing: GHR-04V-S. Pitch: 1.25 mm. Contact: SSHL-002T-P0.2.

- Pin 1: +5V
- Pin 2: EXT_I2C_SCL
- Pin 3: EXT_I2C_SDA
- Pin 4: GND

### J21 — AIRSPEED_I2C4_5V

Header: SM04B-GHS-TB. Cable housing: GHR-04V-S. Pitch: 1.25 mm. Contact: SSHL-002T-P0.2.

- Pin 1: +5V
- Pin 2: EXT_I2C_SCL
- Pin 3: EXT_I2C_SDA
- Pin 4: GND

### J24 — BUZZER_5V_20mA_MAX

Header: SM02B-GHS-TB. Cable housing: GHR-02V-S. Pitch: 1.25 mm. Contact: SSHL-002T-P0.2.

- Pin 1: +5V
- Pin 2: BUZZER_LOW

## Compatibility and limits

- J18 uses the Pixhawk GPS2 six-pin signal order: +5V, controller TX, controller RX, SCL, SDA, GND. It does not use the ten-pin GPS1 safety-switch connector.
- J19 uses the six-pin telemetry power/TX/RX/GND order; pins 4 and 5 are unconnected. Disable RTS/CTS hardware flow control in the connected radio and firmware.
- J17 is a UART receiver port with the same six-pin power/TX/RX/GND order and no flow control. It is not the Pixhawk five-pin PPM/SBUS port or the three-pin DSM port.
- J20 and J21 now supply +5V on pin 1, following the Pixhawk I2C connector convention. Signal pull-ups remain at +3.3V. Connect only modules accepting +5V power with 3.3V I2C logic.
- J18, J20 and J21 share the external I2C4 bus; device addresses must not conflict.
- J1 is a custom five-pin GH SWD connector. Pixhawk FMU debug cables use a different JST-SH connector and pinout. Use the listed SWD pin map.
- J24 retains +5V and switched buzzer return. Use an active buzzer within the existing 20mA design limit.
- J4 regulated power, J22 servo BEC power, OUT1–OUT12, USB-C, microSD and the boot jumper are preserved.
- J23 deliberately retains the original raw battery measurement connector, as requested. Do not substitute JST-GH here: the GH rated voltage is 50V, while a fully charged 12S pack reaches 50.4V. The existing measurement input is not a controller power input.
- JST-GH rating is 1A per contact with AWG26 under the manufacturer conditions. This connector change does not qualify the board for flight or establish the available peripheral supply current.

## Verification

ERC: 0 violations. DRC: 0 violations, 0 unconnected items, 0 schematic parity issues, using the existing project rule configuration. No check severities were relaxed for this revision. All 480 schematic pin assignments match the PCB. Other than the seven connector replacements, all 125 footprint positions were preserved.

## 3D representation

The connector models are the installed KiCad library models. ICM42688 and BMP390 use nominal package envelopes; Y1 uses a generic 5032 crystal model. These are visual placement aids, not manufacturer-qualified mechanical models.

## Sources

- https://docs.holybro.com/autopilot/pixhawk-6c/pixhawk-6c-ports
- https://jst.es/wp-content/uploads/2025/06/gh-connector.pdf

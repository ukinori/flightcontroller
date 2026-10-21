# Interface circuitry and connector guide

English edition. The PCB is now routed; early layout tasks in previous editions have been completed. Refer to the main README and current CAD sources for the publication snapshot.

## External connectors

TX/RX names refer to the controller. Connect controller TX to device RX and controller RX to device TX. A 5 V power pin does not imply 5 V signal tolerance; UART signals are intended for compatible 3.3 V logic.

- J17 RC, USART2: 1 = +5V, 2 = PA2 / RC_TX, 3 = PA3 / RC_RX, 4 = GND. Select a compatible protocol; inverted SBUS and any particular receiver need separate review.
- J18 GPS, USART1: 1 = +5V, 2 = PA9 / GPS_TX, 3 = PA10 / GPS_RX, 4 = GND.
- J19 telemetry, USART3: 1 = +5V, 2 = PB10 / TELEM_TX, 3 = PB11 / TELEM_RX, 4 = GND.
- J20 compass, I2C4: 1 = +3.3V, 2 = PB6 / SCL, 3 = PB9 / SDA, 4 = GND.
- J21 airspeed: same I2C4 bus and pinout as J20. Devices need compatible, non-conflicting addresses. Shared pull-ups are 4.7 kohm to 3.3 V. Devices requiring 5 V power need a separately reviewed power arrangement.
- J22 separate servo BEC: 1 = VCC_SERVO, 2 = GND. Current intended supply: regulated 6 V. Local capacitance: 10 uF plus 470 uF / 16 V. This rail is separate from controller +5V. The current connector is Phoenix Contact 1715721. Validate combined servo stall current and the entire supply path.
- J23 sensing only: 1 = BAT_RAW_SENSE, 2 = external 0–3.3 V analog current-sense output, 3 = GND. This is not a board power input.
- J24 active buzzer: 1 = +5V, 2 = switched BUZZER_LOW. The recorded circuit targets a 5 V active buzzer up to 20 mA: PE10, MMBT3904, 2.2 kohm base resistor, 100 kohm pull-down and flyback diode. This is on/off control, not completed PX4 timer-driven tone_alarm support.

## Outputs

All twelve headers: pin 1 signal, pin 3 GND. Pin 2 is unconnected on OUT1–OUT6 and is VCC_SERVO on OUT7–OUT12.

- OUT1–OUT4: PE9, PE11, PE13, PE14; intended TIM1 channels 1–4.
- OUT5–OUT6: PC6, PC7; intended TIM8 channels 1–2. OUT5 is the cruise-motor allocation and OUT6 is spare.
- OUT7–OUT10: PD12–PD15; intended TIM4 channels 1–4.
- OUT11–OUT12: PA0–PA1; intended TIM2 channels 1–2.

Every output has a 100 ohm series resistor. OUT1–OUT6 have 10 kohm pull-downs. Signals remain at MCU logic voltage. Series resistors do not provide level conversion or complete external overvoltage protection. Verify actual ESC/servo requirements; add suitable buffers if necessary. DShot requires a dedicated PX4 timer/DMA implementation and bench validation. The main README gives the exact connector/GPIO/resistor map.

## Measurement and service circuits

- PC0 / ADC_BAT_V / ADC1_INP10: 91 kohm + 91 kohm / 10 kohm divider, 1% resistors, ratio 19.2. A nominal 50.4 V input gives 2.625 V, followed by 1 kohm / 100 nF filtering. This assumes ordinary 12S cells charged to 4.2 V each. Check tolerances, transient exposure and input protection.
- PC1 / ADC_BAT_I / ADC1_INP11: 1 kohm / 100 nF filter. Current sensor, offset, gain and calibration remain to be selected.
- PC13 / SAFETY_BTN: button to GND, 10 kohm pull-up and 100 nF filter.
- NRST: reset button to GND with the existing capacitor.
- PE0 / LED_STATUS: LED with 1 kohm resistor, active high. A separate LED indicates 3.3 V power.
- USB: USBLC6-2SC6 protection on D+/D-, with protection supply and 100 nF on connector VBUS before the diode. Review line routing and ground return. The schematic uses the compatible-pinout USBLC6-2P6 symbol and an explicit SOT-23-6 footprint for the SC6 part.
- BMI088 uses SPI4 labels for PE2/PE5/PE6. FRAM remains on SPI3.

## Remaining work

Select actual peripherals, verify voltages/protocols/cables, review external-port protection and output buffers, and complete purchasing-part and power checks. The USB protection device does not protect every external connector. Develop a custom PX4 target and perform bench testing. Fresh flight-controller CAD reports are in verification/; the unfinished meshtransciever project is outside that verification scope.

## References

- https://www.st.com/resource/en/datasheet/stm32h743ii.pdf
- https://www.st.com/resource/en/datasheet/usblc6-2.pdf
- https://docs.px4.io/main/en/sensor_bus/i2c_general
- https://docs.px4.io/main/en/peripherals/dshot

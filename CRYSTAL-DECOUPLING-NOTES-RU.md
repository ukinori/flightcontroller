# Crystal and decoupling component selection

English edition of the engineering record dated 2026-10-13. The legacy filename is retained for existing links. Component selections require validation on assembled hardware.

## Selected parts

- C17/C21: 100 nF, 25 V, X7R, ±10%, 0603, Murata GRM188R71E104KA01D. Local VDD decoupling for U2/U3.
- C18/C22: 2.2 uF, 16 V, X7R, ±10%, 0805, Murata GRM21BR71C225KA12L. Local bulk capacitance for U2/U3; account for DC-bias derating.
- C28: 100 nF, 25 V, X7R, ±10%, 0603, Murata GRM188R71E104KA01D. FRAM decoupling, close to U5 pin 8.
- Y1: Abracon ABM3-8.000MHZ-12-D4Y-T; 8 MHz, CL = 12 pF, ESR <= 140 ohms, C0 <= 7 pF, -40 to +85 °C. Frequency tolerance and stability are each ±30 ppm. ABM3 two-pad package, 5 × 3.2 mm.
- C15/C16: 18 pF, 50 V, C0G, ±5%, 0603, Murata GRM1885C1H180JA01D.

## HSE calculations

The recorded load-capacitor estimate is `C15 = C16 = 2 × (CL - Cs)`. With CL = 12 pF and assumed stray capacitance Cs = 3 pF, the starting value is 18 pF. Actual stray capacitance depends on the board and must be checked after assembly.

The critical transconductance calculation is `gmcrit = 4 × ESR × (2πf)^2 × (C0 + CL)^2 = 0.511 mA/V`, using the maximum ESR and C0 above. This is below the referenced STM32H743 Y/V revision limit of 1.5 mA/V. Confirm the applicable silicon revision and datasheet conditions; this preliminary calculation does not replace oscillator testing.

Verify startup across the intended supply/temperature range, measure frequency through MCO, and check drive level against the ABM3 100 uW maximum. Adjust the capacitors and any limiting resistor as required by ST AN2867. A normal oscilloscope probe directly on the crystal alters its load. Keep Y1/C15/C16 close to PH0/PH1, MCU pins 12/13, with short connections and a local ground return.

## References used for selection

- TDK ICM-42688-P, sections 4.2–4.3: https://invensense.tdk.com/wp-content/uploads/2020/04/ds-000347_icm-42688-p-datasheet.pdf
- Exact crystal: https://abracon.com/parametric/crystals/ABM3-8.000MHZ-12-D4Y-T
- ABM3: https://abracon.com/Resonators/ABM3.pdf
- STM32H743: https://www.st.com/resource/en/datasheet/stm32h743vi.pdf
- ST oscillator guidance: https://community.st.com/stm32-mcus-60/how-to-select-a-compatible-crystal-and-load-capacitors-for-stm32-with-layout-guidelines-148169
- Murata load capacitor: https://search.murata.co.jp/Ceramy/image/img/A01X/G101/ENG/GRM1885C1H180JA01-01.pdf

The associated CSV covers these selected parts. Other purchasing parts and the complete PCB/BOM still require review.

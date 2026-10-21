# Flight-controller power design

English edition of the selection record dated 2026-10-12. The legacy filename is retained for links. The onboard circuit converts regulated 5 V to 3.3 V. A separate external converter is required between the intended 6–12S battery and J4. Never connect raw battery voltage to J4.

## Selected circuit

- U8: TPS62162DSGR, fixed 3.3 V, nominal 1 A capability, 3–17 V input. WSON-8 with exposed pad; local symbol FlightController:TPS62162DSG.
- L1: 3.3 uH, Coilcraft XAL4030-332MEC, assigned XAL4030 footprint.
- C34/C42: two parallel 10 uF / 25 V, X7R, ±10%, 1206, Murata GRM31CR71E106KA12L.
- C31/C33: 100 nF / 25 V, X7R, ±10%, 0603, Murata GRM188R71E104KA01D.
- C32: 22 uF / 10 V, X7R, ±10%, 1206, Murata GRM31CR71A226KE15L.
- D3: unidirectional SMBJ5.0A-E3/52, SMB; cathode to PWR_IN, anode to GND. It handles suitable finite-energy transients, not sustained regulator failure or direct battery connection.
- D2/D4: SS14, SMA, combining USB and external supply paths. D1 was removed; USB power passes through the single series diode D2.

The previous TLV757 linear regulator was replaced. GPS, telemetry and output connectors are on the interface sheet.

## Preliminary 3.3 V budget

These are allocated limits, not measured currents:

- STM32H743: 450 mA, based on a referenced 480 MHz datasheet point around 439 mA at Tj = 85 °C; this does not cover every operating mode or temperature.
- microSD: 200 mA peak-write allocation; qualify the selected card.
- External I2C sensors: 100 mA total.
- Two ICM sensors: 3 mA; BMI088: 7 mA; two BMP390 devices: 2 mA; FRAM: 3 mA.
- LEDs and pull resistors: 10 mA.

Total: **775 mA**. The working target is <= 800 mA including validated transient loads. Margin to the nominal 1 A regulator rating does not replace thermal/stability testing.

A linear regulator at 775 mA would dissipate approximately 0.93–1.32 W for a 4.5–5 V input. Measure actual TPS62162 temperature on the assembled board.

Full load is not guaranteed from ordinary USB 2.0: 775 mA at 3.3 V requires more than 500 mA at 5 V after conversion losses. Use adequate external regulated 5 V for the complete system. The circuit does not detect USB-C advertised current. GPS and telemetry power pins use the external +5V rail.

## Capacitor-selection assumptions

This selection covers the local regulator capacitors, not all project capacitors. For C32, the recorded typical Murata curve indicates approximately 25% capacitance loss at 3.3 V. Applying -10% tolerance and -15% temperature allowance gives `22 × 0.75 × 0.90 × 0.85 ≈ 12.6 uF`, above the adopted 10 uF threshold. This is a typical-curve estimate, not a guaranteed minimum; combined effects, ageing and variation require checking.

Each input 10 uF capacitor has approximately 8 uF at 5 V according to the published 25 °C, 1 Vrms curve. Two in parallel give approximately 16 uF, or approximately 12.2 uF with the same tolerance/temperature allowances. Real small-signal conditions differ from curve conditions. Place input capacitors directly between U8 VIN and PGND.

The larger MLCCs use 1206 packages (3.2 × 1.6 mm); 100 nF capacitors use 0603 (1.6 × 0.8 mm). These MLCCs are non-polarized. The original selection did not verify stock or prices.

## Review and testing

1. Match replacements by effective operating capacitance, not nominal capacitance alone.
2. Check package drawings, especially WSON and the inductor.
3. Review CIN/U8/L1/COUT layout against TI guidance: small switching loop, short SW copper, appropriate VOS sensing, exposed pad on GND with vias.
4. Check converter noise coupling into IMUs, barometers and ADC measurements.
5. Use a current-limited bench supply; check voltage, ripple, transients and temperature during SD writes and sensor operation.
6. Qualify the external 5 V converter for total load, current limiting and faults. A TVS is not sustained overvoltage protection.

Fresh ERC/DRC reports are in verification/. Existing ignored checks are listed in the main README. CAD checks do not establish hardware function or flight readiness.

## References used for selection

- https://www.ti.com/lit/ds/symlink/tps62162.pdf
- https://www.ti.com/lit/ds/symlink/tlv757p.pdf
- https://www.st.com/resource/en/datasheet/stm32h743ii.pdf
- https://www.vishay.com/docs/88392/smbj.pdf
- https://www.vishay.com/docs/88746/ss12.pdf
- https://www.coilcraft.com/getmedia/6adcb47d-8b55-416c-976e-1e22e0d2848c/xal4000.pdf
- https://ds.murata.com/simsurfing/mlcc.html?partnumbers=%5B%22GRM31CR71E106KA12%22%5D
- https://ds.murata.com/simsurfing/mlcc.html?partnumbers=%5B%22GRM31CR71A226KE15%22%5D
- C32 curve, page 3: https://www.farnell.com/datasheets/3412299.pdf
- Input-capacitor curve, page 2: https://akizukidenshi.com/goodsaffix/GRM31CR71E106KA12.pdf
- https://www.murata.com/en-eu/api/pdfdownloadapi?cate=luCeramicCapacitorsSMD&partno=GRM188R71E104KA01%23

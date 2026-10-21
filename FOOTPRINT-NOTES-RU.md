# Footprint assignment notes

English edition; the legacy filename is retained for existing links. Footprint assignments are CAD decisions, not a complete qualified procurement BOM.

Physical symbols have footprint assignments; symbol pin numbers were checked against pad numbers. Verify actual purchasing parts against manufacturer drawings before fabrication.

- Output, GPS, telemetry, SWD and BOOT0 connectors use 2.54 mm vertical headers. The assignment does not establish compatibility with any particular cable.
- J2: GCT USB4105-GF-A. Unused SBU contacts A8/B8 remain unconnected.
- J3: Hirose DM3AT-SF-PEJM5. Detection contacts 9/10 remain unconnected.
- U5: MB85RS64PNF-G-JNE1, SOP-8, 150 mil body.
- FB1: Murata BLM18PG121SN1D, 0603.
- Y1: ABM3-8.000MHZ-12-D4Y-T, CL = 12 pF; C15/C16 = 18 pF. See CRYSTAL-DECOUPLING-NOTES-RU.md for assumptions and validation.
- C17/C21/C28 = 100 nF; C18/C22 = 2.2 uF. Parts and packages are in CRYSTAL-DECOUPLING-BOM.csv.
- C36: select an actual 470 uF / 16 V electrolytic matching the assigned 8 mm diameter and 3.5 mm lead pitch.
- J22: the later routed board uses Phoenix Contact 1715721 for the separate servo-power input.

Remaining generic passive packages must be matched to actual parts. FOOTPRINT-ASSIGNMENTS.csv is an assignment record rather than a manufacturing-ready BOM.

## References

- https://gct.co/connector/usb4105
- https://www.hirose.com/product/p/CL0609-0031-0-00
- https://www.ramxeed.com/products/feram/s4.html
- https://www.murata.com/products/productdata/8796738650142/ENFA0003.pdf
- https://abracon.com/Resonators/ABM3.pdf

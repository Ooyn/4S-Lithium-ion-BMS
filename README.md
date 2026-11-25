# 4S Lithium-Ion Active Shunt Balancing BMS

Custom 4-cell (16.8V) Lithium-Ion Battery Management System PCB designed in **KiCad**, featuring an analog active shunt cell-balancing topology, reverse-polarity input protection, and adjustable constant-voltage regulation.

![Assembled 4S BMS PCB](docs/assembled_board.jpg)

---

## Key Specifications

| Parameter | Specification |
| :--- | :--- |
| **Battery Configuration** | 4S Lithium-Ion (14.8V Nominal / 16.8V Max) |
| **Balancing Topology** | Analog Active Shunt Bypass (Per-Cell Independent) |
| **Cell Cutoff Threshold** | 4.20V per cell |
| **Voltage Reference / Comparator** | TL431LP Precision Programmable Shunt Regulator |
| **Bypass Switch** | BD140 PNP Power Transistor + Series 1N4007 Diodes |
| **Input Stage & Regulation** | Reverse-Polarity MOSFET Protection + LM317 Adjustable Regulator |
| **EDA Tool** | KiCad (`Charger.kicad_sch`, `Charger.kicad_pcb`) |

---

## Hardware Architecture

1. **Input Protection & Voltage Regulation:**
   - A series MOSFET at the power input guards against reverse-polarity connections.
   - An **LM317** adjustable linear regulator paired with a trimmer potentiometer and decoupling capacitors sets the pack charging voltage to **16.8V** (`4 × 4.2V`).
2. **Per-Cell Voltage Sensing (`TL431LP`):**
   - Four identical balancing channels monitor each series cell independently using a precision resistor divider connected to the **TL431LP** reference pin ($V_{\text{ref}} \approx 2.495\text{V}$).
   - The divider ratio sets the conduction threshold at a cell voltage of **4.20V**:
     $$V_{\text{cell}} = V_{\text{ref}} \left(1 + \frac{R_1}{R_2}\right) \approx 4.20\text{V}$$
3. **Current Shunt Bypass (`BD140` + `1N4007`):**
   - Once a cell reaches **4.20V**, its TL431LP conducts and pulls base current from the paired **BD140** PNP transistor.
   - The BD140 turns on and routes excess charging current through a series array of **1N4007** diodes, dissipating energy safely while allowing under-charged cells in the series string to continue charging.
4. **Visual Status Indicators:**
   - Dedicated per-channel LEDs illuminate whenever a cell's shunt bypass path is active.

---

## Bill of Materials (Key Components)

| Designator | Component | Role |
| :--- | :--- | :--- |
| `U1 – U4` | **TL431LP** | Precision voltage reference & threshold comparator |
| `Q2 – Q4` (4x) | **BD140** | PNP power transistor for cell bypass switching |
| `U5` | **LM317** | Adjustable pack voltage regulator (16.8V output) |
| `Q1` | **Power MOSFET** | Reverse-polarity input protection |
| `D1 – D22` | **1N4007 / LEDs** | Bypass dissipation diode array & active-balance indicators |

---

## Repository Contents

- `hardware/kicad/` — Native KiCad schematic (`Charger.kicad_sch`) and PCB layout (`Charger.kicad_pcb`) files.
- `hardware/manufacturing/` — Gerber fabrication files and drill outputs.
- `docs/schematic.pdf` — Printable schematic PDF for quick review.

---

## Bench Bring-Up & Validation Notes

- **Trim Calibration:** Adjusted the LM317 feedback potentiometer under no-load and simulated 4S load conditions to lock the charging rail at `16.80V`.
- **Threshold Verification:** Tested individual cell channels using a bench power supply to verify TL431LP trigger accuracy, BD140 thermal behavior during shunt conduction, and LED indicator activation at `4.20V`.

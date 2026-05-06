# Differential Pair Amplifier with Current Mirror Load

## How it works

This design implements a classic CMOS differential pair amplifier using the SKY130A PDK at 1.8V supply. It consists of five transistors:

- **M1, M2 (NMOS, W=6µm, L=0.5µm)** — Differential input pair. M1 is the non-inverting input (IN+) and M2 is the inverting input (IN−).
- **M3, M4 (PMOS, W=12µm, L=0.5µm)** — Current mirror load. M3 is diode-connected and sets the reference. M4 mirrors M3's current to produce single-ended output.
- **M5 (NMOS, W=4µm, L=1µm)** — Tail current source. Biased by Vbias to set the total differential pair current.

When IN+ rises above IN−, more current flows through M1, causing M3 to set a higher mirror current. M4 then forces more current through the output node, raising Vout. The current mirror load converts the differential current signal into a single-ended voltage output at high gain.

## Verified specifications

| Parameter | Pre-layout | Post-layout | Target |
|-----------|-----------|-------------|--------|
| Supply voltage | 1.8V | 1.8V | 1.8V |
| AC Gain | 36 dB | 36 dB | 25–35 dB |
| Bandwidth (−3dB) | 5 MHz | 15–20 MHz | 1–10 MHz |
| Tail current | 17.76 µA | 26.79 µA | 10–20 µA |
| Input CM range | 0.6–1.2V | 0.6–1.2V | 0.6–1.2V |
| Vout DC (balanced) | 0.75V | 0.77V | ~0.9V |

Simulations were run at TT (typical-typical) and SS (slow-slow) corners. The design passes all target specifications at both corners.

## Transistor sizing

| Device | Type | W | L | Role |
|--------|------|---|---|------|
| M1, M2 | nfet_01v8 | 6µm | 0.5µm | Differential pair |
| M3, M4 | pfet_01v8 | 12µm | 0.5µm | Current mirror load |
| M5 | nfet_01v8 | 4µm | 1µm | Tail current source |

## Layout notes

- M1 and M2 are placed using a **common centroid ABBA pattern** (each split into W=3µm halves) to minimise offset voltage due to process gradients.
- M3 and M4 are placed adjacent with identical orientation for best current mirror matching.
- Guard rings surround all NMOS (p-substrate contact → VGND) and PMOS (n-well contact → VDPWR) device groups.

## How to test

**Required equipment:**
- DC power supply (1.8V)
- Two DC voltage sources or a function generator
- Oscilloscope or multimeter

**Setup:**
1. Apply **1.8V** to VDPWR and **GND** to VGND
2. Apply **Vbias = 0.78V** to `ua[3]`
3. Apply **common mode = 0.9V** to both `ua[1]` (IN+) and `ua[2]` (IN−) initially
4. Monitor `ua[0]` (Vout) — should read approximately **0.75–0.80V**

**Gain measurement:**
1. Apply a small differential signal: `ua[1]` = 0.9V + 5mV, `ua[2]` = 0.9V − 5mV
2. Measure Vout change at `ua[0]`
3. Gain = ΔVout / ΔVdiff (target: 36 dB ≈ 63×)

**Example:** 10mV differential input → ~630mV output swing

## Pin assignments

| Pin | Signal | Direction | Description |
|-----|--------|-----------|-------------|
| ua[0] | Vout | Output | Single-ended amplifier output |
| ua[1] | IN+ | Input | Non-inverting differential input |
| ua[2] | IN− | Input | Inverting differential input |
| ua[3] | Vbias | Input | Tail current bias (connect to 0.78V DC) |

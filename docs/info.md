# Differential Pair Amplifier
Differential pair Amplifier

## How it works
NMOS differential pair (M1,M2) with PMOS current mirror load (M3,M4)
and NMOS tail current source (M5).

## Specifications
| Parameter    | Pre-layout | Post-layout |
|-------------|-----------|-------------|
| Supply      | 1.8V      | 1.8V        |
| Gain        | 36 dB     | 36 dB       |
| Bandwidth   | 5 MHz     | 15-20 MHz   |
| Tail current| 17.76 µA  | 26.79 µA    |

## How to test
Apply 1.8V to VDPWR, GND to VGND.
Apply Vbias=0.78V to ua[3].
Apply differential signal to ua[1] and ua[2].
Measure output at ua[0].

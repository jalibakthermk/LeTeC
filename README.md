# LeTeC – LED Temperature Calculator

A quick, single-page web tool to estimate the **junction temperature (Tj)** of LEDs from a few datasheet values.

Open `index.html` in any browser. No install and no build step needed. It also works on GitHub Pages. The look comes from the shared [beamKit](https://jalibakthermk.github.io/beamkit/demo.html) design framework, loaded from the portfolio site; offline the calculator still works, with plain fallback styling.

## Inputs

| Control | Parameter |
|---|---|
| Knob | Ambient temperature (°C) |
| Knob | Light (radiant) efficiency (%) |
| Knob | Maximum junction temperature (°C) |
| Slider | Number of LEDs |
| Slider | Current per LED (mA) |
| Slider | Forward voltage (V) |
| Slider | Rth junction–solder point (K/W) |
| Slider | Board size (cm²), for still-air cooling |
| Slider | Heat sink Rth (K/W), for heat-sink cooling |

Every control also has a box for typing exact values.

## Outputs

- Junction temperature with an OK / close / too-hot status and a gauge against the limit
- Board temperature, heat to remove, electrical power
- Still air: heat transfer coefficient, board-to-air Rth, and the board size needed to stay at the limit
- Heat sink: the maximum heat-sink Rth allowed

## Model

```
Q  = N × I × Vf × (1 − efficiency)
Tj = Ta + Q × R_cooling + (Q / N) × Rth,j-sp
```

- **Still air (no heat sink):** the board cools by natural convection (laminar vertical-plate correlation, h_c = 1.42 (ΔT/L)^0.25) plus radiation (ε = 0.9), solved iteratively because h depends on the temperature rise.
- **Heat sink:** uses the heat sink's Rth from its datasheet, a measurement or a simulation.

Accuracy is about ±20 %. An LED inside a closed housing will run hotter than the still-air result.

## Where to find the values

- **Rth junction–solder point:** LED datasheet, "Thermal" table (Rth JS). Use the *real* value, not the electrical one.
- **Heat sink Rth:** heat-sink catalogue (natural convection, usually quoted at ΔT ≈ 75 K), or measure it as (T_sink − T_ambient) / Q.

# picorv32 — RTL-to-GDSII Tapeout & Multi-Clock PPA Characterization

**[Live results dashboard →](https://asif7865651.github.io/picorv32-sky130-ppa-dashboard/)**

Full synthesis-to-GDSII physical design flow for a [PicoRV32](https://github.com/YosysHQ/picorv32) RISC-V core, run on [OpenLane](https://github.com/The-OpenROAD-Project/OpenLane) targeting the open-source [Sky130](https://github.com/google/skywater-pdk) PDK — characterized across three clock targets rather than a single pass/fail run, with a self-built results dashboard and rendered chip layout.

## Results at a glance

| Clock Target | Status | Utilization | WNS | Actual Critical Path | DRC / Routing Violations |
|---|---|---|---|---|---|
| 25ns (baseline) | Flow completed | 46.18% | 0 ns | 1.5 ns | 0 / 0 |
| 20ns | Flow completed | 46.18% | 0 ns | 6.0 ns | 0 / 0 |
| 15ns | Flow completed | 46.18% | 0 ns | 5.77 ns | 0 / 0 |

All three clock targets closed timing cleanly with zero DRC, LVS, and routing violations — meaning the design's true frequency ceiling is higher than any target tested here.

- **Die area:** 0.254 mm² (487.6 × 486.88 µm)
- **Synthesized cells:** 11,193 &nbsp;|&nbsp; **Total physical cells:** 32,197
- **Antenna violations found:** 52 pin, 38 net (partially auto-repaired via diode insertion — a genuine finding, not a clean-slate run)

## What's in this repo

- `index.html` — the full static results dashboard (also served live via GitHub Pages)
- `chip_layout.png` — full-die render of the final signed-off GDSII
- `chip_layout_zoom.png` — zoomed detail (25×25 µm) showing individual standard-cell rows and routing

## Flow

Synthesis (Yosys) → Floorplan → Placement → Clock Tree Synthesis → Routing (TritonRoute) → Signoff (multi-corner STA, DRC via Magic, LVS, antenna/ERC checks, GDSII export cross-verified between Magic and KLayout).

## Tools

OpenLane · Yosys · OpenROAD · Magic · KLayout · Sky130 PDK · Python (custom dashboard + static site generator, standard library only, no external dependencies)

## Why three clock targets

Rather than a single run, the same design was re-synthesized at three clock periods (25ns / 20ns / 15ns), changing only `CLOCK_PERIOD` and nothing else in the config — a small design-space exploration to find where timing closure actually starts to break, and to report the real measured critical path rather than the nominal clock target.

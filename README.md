# Home Printing Instructions

This repository contains 3D print files and settings for home printing. Use this README as a quick guide for slicing and printing the included models.

## Quick start
1. Open the desired model (.stl/.3mf) in your slicer (Cura, PrusaSlicer, PrusaSlicer, Slic3r, Simplify3D).
2. Choose the material and printer profile closest to your hardware.
3. Apply the settings below as a baseline and adjust for your printer.

## Recommended baseline settings
- Layer height: 0.2 mm (0.12–0.3 mm depending on detail vs speed)
- Nozzle: 0.4 mm
- Print speed: 40–60 mm/s (lower for small detailed parts)
- Infill: 15–25% (20% is a good general-purpose value)
- Walls/perimeters: 2–3
- Top solid layers: 4–6
- Bottom solid layers: 4–6
- Retraction: enable (start: 1 mm, speed: 25–45 mm/s) — tune for Bowden vs direct drive

## Temperature (common filaments)
- PLA: nozzle 200°C, bed 50–60°C
- PETG: nozzle 235–250°C, bed 70–85°C
- ABS: nozzle 230–250°C, bed 90–110°C (use enclosure)
- TPU: nozzle 210–230°C, bed 30–50°C (slow speeds, direct drive recommended)

Adjust temperatures based on manufacturer filament datasheet and your printer's calibration.

## Bed adhesion
- Use a clean, leveled bed. Re-level if you change nozzle or remove/replace the bed surface.
- Use glue stick, blue painter's tape, PEI, or BuildTak as appropriate.
- Brims for small bases or thin parts; rafts if the model warps or requires stronger adhesion.

## Supports and orientation
- Orient parts to minimize overhangs and supports where practical.
- Use supports for overhangs > 60° or where small features would droop.
- For cosmetic surfaces, use tree supports or lower support density and increase support Z-distance.

## Post-processing
- Remove supports carefully with flush cutters or pliers.
- Sanding: start with 200–400 grit and move to 800–2000 for smooth finish.
- For PLA smoothing, consider light sanding and polishing; for ABS acetone vapor smoothing works (use caution).

## Troubleshooting
- First layer not sticking: re-level bed, increase first-layer temperature, slow down first layer, increase first-layer height/flow.
- Stringing: increase retraction distance/speed, lower print temperature.
- Warping: increase bed temp, add enclosure, use brim/raft.
- Elephant's foot: reduce first layer Z offset, or add a skirt only.

## Slicer profile files
If slicer profiles are included in this repo, import them into your slicer and verify the printer/filament settings before printing.

## Licensing and credits
Check each model's header or the repository LICENSE file for licensing terms. Credit original designers where applicable.

## Contact / Feedback
If you find issues with prints or need recommended settings for a specific printer, open an issue in this repository with details (printer model, filament, symptom, and pictures if possible).

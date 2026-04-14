# Deliverables (MATLAB + Word)

## Files
- `deliverables/obc_deliverables_simulation.m` — MATLAB analytical simulation for deliverable metrics.
- `deliverables/OBC_Deliverables.docx` — Word file containing only the requested deliverables sections.
- `deliverables/outputs/` — generated simulation outputs after MATLAB run.

## MATLAB run steps
1. Open MATLAB.
2. Run:
   ```matlab
   cd('.../nextjs-boilerplate/deliverables')
   obc_deliverables_simulation
   ```
3. Check `deliverables/outputs/` for CSV/MAT/PNG outputs.

## What the simulation covers
- Efficiency curve across load range (10% to 100%).
- Input power factor across load range.
- Output voltage regulation across load range.
- Dynamic output-voltage response for 30% to 100% load step.
- Automatic compliance checks for:
  - Efficiency > 95%
  - Power factor > 0.98
  - Voltage regulation within ±1%

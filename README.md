# Methacrylate Monolith Simulator

Interactive desktop simulator for methacrylate monolith design, synthesis recipe guidance, pressure-drop prediction, pore-distribution exploration, and 3D visualization.

## What This Simulator Does

- Creates and edits methacrylate monolith geometries.
- Predicts back pressure (rigid and compression-aware).
- Calculates bimodal pore distribution (macro + mesopores).
- Generates synthesis recipes (GMA-EDMA) with:
  - composition recommendations,
  - polymerization temperature guidance,
  - polymerization (reaction) time estimates,
  - heat-transfer and risk notes.
- **Optional flexible recipe mode**: load monomer/porogen structures (`.mol`, `.sdf`, `.cml` from Avogadro, GAMESS exports, etc.); **RDKit** estimates simple HSP parameters, HSP distance Ra, pore-size modifiers, and a short feasibility narrative. This is a **theory-informed screening tool**, not a guarantee of lab outcomes.
- Visualizes monolith structure in embedded 3D and standalone 3D views.
- Supports optional Professional Judgment Mode for large monolith recipe checks.

### Predictions vs reality

Pressure drop, pore statistics, recipe volumes, and flexible-mode HSP estimates are **educated models** (literature-style correlations, heuristics, and scaling rules). They help explore design space and communicate reasoning; they **do not replace** experiments, SDS review, or your institution’s safety and validation requirements.

## Main Menus (How To Navigate)

## 1) Objects

- **Create methacrylate monolith...**
  - Define porosity, pore size, and geometry.
- **Edit monolith...**
  - Modify an existing monolith.

Tip: Most calculations require an active monolith first.

## 2) Calculate

- **Predict back pressure drop...**
  - Uses monolith and sample/flow parameters.
- **Bimodal distribution**
  - Computes macro/mesopore distribution and updates porous view data.
- **Synthesis recipe (GMA-EDMA)...**
  - Opens recipe input dialog for process planning (standard GMA–EDMA or **flexible** structure-file mode if RDKit is installed).

## 3) View

- Toggle bottom bar and coordinate widget.
- Toggle custom cursor.
- Open 3D views (solid/porous/SEM-based).
- Toggle radial/axial/radial-3D flow panels.
- Toggle X-ray porous view.

## 4) Animate

- Open animated radial/axial flow panels.

---

## Quick Start

1. Go to **Objects -> Create methacrylate monolith...**
2. Set realistic geometry and pore parameters.
3. Run **Calculate -> Bimodal distribution** (recommended before porous/X-ray visualization).
4. Run **Calculate -> Synthesis recipe (GMA-EDMA)...**
5. (Optional) Enable **Professional Judgment Mode** for large monolith checks.
6. Explore **View** and **Animate** menus for 3D and flow behavior.

---

## Recipe Dialog Guide

In the recipe input window:

- **Mould fill volume (mL = cm^3)**: total mixture volume.
- **Target avg. pore size (um), optional**
- **Target porosity (%), optional**
- **Atmosphere**: N2 or Air
- **Optimization bias**:
  - **Balanced**: compromise between pore-size and porosity targets.
  - **Porosity priority**: favors matching porosity target.
  - **Pore size priority**: favors matching pore-size target.
- **Use active monolith** button:
  - Auto-fills volume, pore size, and porosity from active monolith.

If no active monolith exists, the app reminds you to create/select one first.

### Flexible recipe mode (optional)

1. Install RDKit, e.g. `pip install -r requirements_recipe_flexible.txt`.
2. In the recipe dialog, choose **Flexible (HSP from .mol / .sdf / .cml)**.
3. Add one or more monomer files and porogen files, set volume fractions **within each phase** (they are renormalized), and optional crosslinker fraction / viscosity ratio.
4. Read the **feasibility** and **explanation** blocks in the results: they summarize Ra, modifiers, and limitations.

---

## Recipe Output Notes

- **Polymerization temperature** is constrained to a practical process window.
- **Polymerization (reaction) time** is reported as hold time at set temperature.
- The time estimate assumes reasonably uniform heating in the mould.
- Adiabatic temperature-rise notes are displayed with a stated conversion assumption.
- For large monoliths, extra heat-management guidance is shown.

---

## Professional Judgment Mode (Recipe Window)

For large monoliths (>20 mL):

- Presents heat/wall precaution options (dual heating, dual cooling, thin walls).
- Window can enforce precaution selection before proceeding.
- Includes judgment summary and warnings.

Neutral mode remains informational-only (no enforcement).

---

## Visualization Notes

- **X-ray porous view** requires porous mesh data.
  - Run **Calculate -> Bimodal distribution** first.
- During heavy resizing/dragging, redraws may be temporarily throttled for smoother UI.

---

## Troubleshooting

- **No active monolith** errors:
  - Create/select a monolith first from **Objects**.
- **X-ray option does nothing**:
  - Ensure bimodal distribution has been calculated.
- **Custom cursor not visible where expected**:
  - It is intentionally disabled in UI chrome zones (menu/title/edges) to preserve native window interactions.
- **Values seem like compromises**:
  - If both porosity and pore-size targets are set, adjust **Optimization bias**.

---

## Files

- `Monolith_GUI.py` - main Tkinter application
- `monolith_recipe.py` - recipe and risk logic
- `monolith_3d.py` - 3D geometry/porous generation/visualization
- `Monolith simulator.py` - console-oriented model script (fallback mode if GUI fails or behaves unexpectedly)

## Console Fallback

If the GUI fails to open, freezes, or behaves unexpectedly on your system, use:

- `Monolith simulator.py`

This script provides a console-based fallback path for running core model calculations and reviewing results without the graphical interface.

## Dependencies (for EXE packaging)

For packaging this project with `auto-py-to-exe` / PyInstaller, install:

- Required
  - `numpy`
  - `matplotlib`
  - `Pillow`
- Needed for 3D monolith windows
  - `pyvista`
- Optional but recommended (better SEM smoothing/morphology fallbacks)
  - `scipy`

Notes:

- `tkinter` is part of standard Python on Windows installers (no pip package needed).
- `auto-py-to-exe` is a packaging tool (development dependency), not a runtime dependency.
- `pyinstaller` is installed automatically with `auto-py-to-exe` in most setups.

Suggested install command:

`pip install numpy matplotlib Pillow pyvista scipy auto-py-to-exe`

## Logging + auto-cleanup (6 months)

You do **not** need an external logging package; use Python stdlib `logging`.

Recommended approach:

- Write logs to a dedicated folder (e.g. `%LOCALAPPDATA%/MethacrylateMonolithSimulator/logs`).
- Rotate daily with `TimedRotatingFileHandler`.
- Keep `backupCount=180` (about 6 months), so older logs are deleted automatically.

Example:

```python
import logging
import os
from logging.handlers import TimedRotatingFileHandler

def setup_app_logger():
    log_dir = os.path.join(os.getenv("LOCALAPPDATA", "."), "MethacrylateMonolithSimulator", "logs")
    os.makedirs(log_dir, exist_ok=True)
    log_path = os.path.join(log_dir, "app.log")

    logger = logging.getLogger("monolith_app")
    logger.setLevel(logging.INFO)

    handler = TimedRotatingFileHandler(
        log_path,
        when="D",          # rotate daily
        interval=1,
        backupCount=180,   # keep ~6 months
        encoding="utf-8",
    )
    fmt = logging.Formatter("%(asctime)s | %(levelname)s | %(name)s | %(message)s")
    handler.setFormatter(fmt)
    logger.addHandler(handler)
    return logger
```

Log examples:

- app startup/shutdown
- recipe generation request + success/failure
- 3D view load errors (missing PyVista, missing files)
- cursor/SEM module load warnings

Current implementation status:

- The GUI now writes logs automatically to:
  - `%LOCALAPPDATA%\MethacrylateMonolithSimulator\logs\app.log`
- Rotation/retention:
  - daily rotation
  - keeps ~180 backups (~6 months), then older logs are deleted automatically
- Logged events include:
  - app startup/shutdown
  - recipe generation start/success/failure
  - unhandled Tk callback exceptions (with traceback)

Simulator scope (for user-facing release context)
-----------------------------------------------
This simulator integrates a methacrylate-monolith mathematics core with physical
constraints and simulation workflows (pressure-drop/compression behavior,
monolith geometry and pore-structure calculations, and axial/radial flow tools)
based on the peer-reviewed literature below.

References (APA 7th)
--------------------
High Purity New England. (2021, March). High Purit-general presentation [PowerPoint slides]. https://hp-ne.com/wp-content/uploads/2021/03/High_Purit-general_presentation.pdf

Korzhikova-Vlakh, E. G., & Tennikova, T. B. (2021). Some factors affecting pore size in the synthesis of rigid polymer monoliths: Theory and its applicability. Journal of Applied Polymer Science, 139(1). https://doi.org/10.1002/app.51431

Merhar, M., Podgornik, A., Barut, M., Zigon, M., & Strancar, A. (2003). Methacrylate monoliths prepared from various hydrophobic and hydrophilic monomers - Structural and chromatographic characteristics. Journal of Separation Science, 26, 322-330. https://doi.org/10.1002/jssc.200390038

Mihelic, I., Krajnc, M., Koloini, T., & Podgornik, A. (2001). Kinetic model of a methacrylate-based monolith polymerization. Industrial & Engineering Chemistry Research, 40(16), 3495-3501. https://doi.org/10.1021/ie010146m

Podgornik, A., Barut, M., Strancar, A., Josic, D., & Koloini, T. (2000). Construction of large-volume monolithic columns. Analytical Chemistry, 72(22), 5693-5699. https://doi.org/10.1021/ac000680o

Podgornik, A., Savnik, A., Jancar, J., & Lendero Krajnc, N. (2014). Design of monoliths through their mechanical properties. Journal of Chromatography A, 1333, 9-17. https://doi.org/10.1016/j.chroma.2014.01.038

Svec, F., & Frechet, J. M. J. (1992). Continuous rods of macroporous polymer as high-performance liquid chromatography separation media. Analytical Chemistry, 64(7), 820-822. https://doi.org/10.1021/ac00031a022

Svec, F., & Frechet, J. M. J. (1995). Temperature, a simple and efficient tool for the control of pore size distribution in macroporous polymers. Macromolecules, 28(22), 7580-7582. https://doi.org/10.1021/ma00126a044

Yang, C., Wei, Y., Zhang, Q., Zhang, W., Li, T., Hu, H., & Zhang, Y. (2005). Preparation and evaluation of a large-volume radial flow monolithic column. Talanta, 66(2), 472-478. https://doi.org/10.1016/j.talanta.2004.09.027


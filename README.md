# Methacrylate Monolith Simulator
(Latest release: v.donut1.0.0-beta. See Releases section) 

Desktop app for quickly exploring methacrylate monolith design, pressure behavior, pore structure, and synthesis recipe guidance.

This guide assumes the app is already installed on Windows (packaged EXE, console hidden), like a normal desktop application.

## Quick Start (2-3 minutes)

1. Launch **Methacrylate Monolith Simulator** from the Start menu or desktop shortcut.
2. Go to `Build -> Methacrylate monolith...`.
3. Enter geometry + pore settings, then create the monolith.
4. Run `Analysis -> Bimodal distributions` (recommended before porous/X-ray views).
5. Then use either:
   - `Analysis -> Back pressure drop prediction...`, or
   - `Tools -> Synthesis recipe (GMA–EDMA)`
6. Explore `View`, `Analysis`, and `Tools` for 3D/flow/recipe features.
7. Use the toolbar `Graphs` button to open the mechanical property graph tool.

Tip: Most calculations need an active monolith first. Flow/porous features also require porosity greater than 0.

## What You Can Do

- Build/edit monolith geometry.
- Predict back pressure (rigid vs compressed pathway model).
- Calculate bimodal pore distribution (macro + mesopore).
- Generate synthesis recipes with:
  - composition guidance,
  - polymerization temperature,
  - polymerization (reaction) time estimate,
  - thermal/risk notes for larger volumes.
- Optionally use flexible recipe mode (RDKit) from structure files (`.mol`, `.sdf`, `.cml`).
- Graph raw data and export as editable files or images. 
- Export and view p(GMA-co-EDMA) crosslink pattern (simulated to a limited extent, might not match experimental reality) in molecular editors such as Avogadro and/or ChemDoodle. 
- Exports and imports to save your projects and interact with other programs. 


## Interface Layout (Where to Find Features)

### 1) Menu Bar (top of app window)

- **File**
  - Project: `New Project`, `Open Project...`, `Save`, `Save As...`, `Close Project`, `Exit`
  - Import: `Molecule (.mol, .sdf)`, `Monolith (.pore)`
  - Export: `Monolith (.pore)`, `Results (.csv)`, `Structured Data (.json)`, `Visualization (.vtk)`, `Export monolithic backbone...`
  - Graph export: `Export -> Mechanical property graph...` (PNG/JPEG/SVG/CSV/XLSX) when a graph window is open
- **Edit**
  - `Undo`, `Redo`, `Delete`, `Edit current monolith`, `Clear log...`, `Clear all...`
  - Shortcuts: `Ctrl+Z`, `Ctrl+Y`, `Ctrl+Delete`
- **Build**
  - `Methacrylate monolith...`
- **Analysis**
  - `Back pressure drop prediction...`, `Bimodal distributions`
  - `2D radial flow`, `2D axial flow`, `3D radial flow`
- **View**
  - Toggle bottom bar, coordinate axes, flow panels, fullscreen, and custom cursor
  - `Reset view` options (`Reset rotation`, `Reset zoom`, `Recenter framing`) + `Reset all`
  - Toggle `Show/Hide 3D porous view` and `Show/Hide X-ray porous view`
- **Tools**
  - `Pores from SEM images (PyVista)...`
  - `Synthesis recipe (GMA–EDMA)`
  - `View 3D monolith (PyVista)...`

### 2) Toolbar (under the menu bar)

- **Undo / Redo icons**
  - Same actions as `Edit -> Undo/Redo`.
- **Zoom In / Zoom Out icons**
  - Change zoom in the embedded 3D monolith view.
- **Navigation icon**
  - Orbit/inspect mode for the 3D view (rotation).
- **Manipulation icon**
  - Drag/move mode for monolith interaction.
- **Graphs icon**
  - Opens the mechanical property graph tool (CSV/Excel import plotting workflow, requires dataset).
  - After opening a graph, use `File -> Export -> Mechanical property graph...` for image/data export.

### 3) Bottom Bar

- Located below the main 3D view.
- Can be shown/hidden from `View -> Show/Hide bottom bar`.
- Used for quick context and selection support (including monolith selection/status area).
- Works together with the coordinate-axes area for orientation feedback.

### 4) Coordinate Axes Area

- Can be shown/hidden from `View -> Show/Hide coordinate axes`.
- Helps you track orientation while rotating/zooming the 3D model.
- Typical interaction flow:
  - rotate/inspect in the 3D panel,
  - use `View -> Reset view` options when needed (`Reset rotation`, `Reset zoom`, `Recenter framing`).

### 5) Flow Channels and Flow Panels

- Open from `Analysis`:
  - `2D radial flow`
  - `2D axial flow`
  - `3D radial flow`
- Show/hide flow panels from `View -> Show/Hide flow panels`.
- Flow panels are interactive and update their own flow visuals/settings.
- `3D radial flow` is rendered on the solid monolith surface (if X-ray view is on, switch it off first).

### 6) Porous, X-ray, and SEM/PyVista Views

- **Embedded porous/X-ray toggles**
  - `View -> Show/Hide 3D porous view`
  - `View -> Show/Hide X-ray porous view`
  - If unavailable, run `Analysis -> Bimodal distributions` first to build porous mesh data (required for porous/X-ray rendering).
- **PyVista windows (separate 3D viewers)**
  - `Tools -> View 3D monolith (PyVista)...` for standalone 3D viewing.
  - `Tools -> Synthesis recipe (GMA–EDMA)`: generates recipes and suggested procedures for monolith preparation. See below for more. 
  - `Tools -> Pores from SEM images (PyVista)...` to generate pores from SEM images and open a 3D PyVista view.
- **SEM-based workflow**
  - Uses SEM image input to derive pore structure, then opens a PyVista visualization for exploration.


## Recipe Tool in 30 Seconds
In `Tools -> Synthesis recipe (GMA–EDMA)`: 
- Set mould volume.
- Optionally set pore-size and/or porosity targets.
- Pick atmosphere (`N2` or `Air`).
- Choose optimization bias:
  - `Balanced`
  - `Porosity priority`
  - `Pore size priority`
- Use `Use active monolith` to auto-fill values when available.

Results report polymerization temperature and polymerization (reaction) time, with assumptions and safety-oriented notes.

## Flexible Recipe Mode (Optional)

Use this if you want to screen custom chemistries instead of fixed GMA-EDMA inputs.

1. Open the recipe dialog and select flexible mode.
2. Load monomer/porogen structure files and set phase volume fractions.
3. Review feasibility/explanation notes in results.

If flexible mode is unavailable in your installed app, contact the app provider/build maintainer for an edition that includes RDKit support.

Note: flexible mode is a theory-informed screening aid, not a lab guarantee.


## Key Files

- `Methacrylate monolith simulator.exe` - launcher/entrypoint used for the app

- Console-based fall back in case the user interface (GUI) fails to launch: 'Monolith simulator.py' 

## Troubleshooting

- **No active monolith**: create one first in `Build -> Methacrylate monolith...`.
- **Flow/porous features are disabled**: ensure a monolith is active and porosity is greater than 0.
- **X-ray or 3D porous view not updating**: run `Analysis -> Bimodal distributions` first (required).
- **App does not start**: reinstall the app, then launch again from Start menu.
- **Feature missing (for example flexible mode)**: your installed build may not include optional components.
- **Mechanical graph export disabled**: open a graph first using the toolbar `Graphs` button.

## Safety and Model Scope

This simulator provides useful engineering-style estimates. Final process choices still need experimental validation, SDS review, and local safety procedures.
The simulator is built on assumptions and mathematical models for methacrylate monolith systems and may not transfer reliably to other monolith chemistries.
Local regulations of waste handling should always be followed.

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

ATTRIBUTIONS
----------------
Finish page image (Setup exe):
Title: A three-headed eagle in a crowned alchemical flask (from Splendor Solis tradition)
Source: Wellcome Collection via Wikimedia Commons
URL: https://commons.wikimedia.org/wiki/File:A_three-headed_eagle_in_a_crowned_alchemical_flask,_represen_Wellcome_V0025636.jpg
Author: Wellcome Collection
License: Creative Commons Attribution 4.0 International (CC BY 4.0)
https://creativecommons.org/licenses/by/4.0/

NOTES FOR USERS
-------------------
- This file is intended as a plain-text, Notepad-friendly reference list (APA 7th).
- Companion documents in the same folder:
  - README.md ............... feature overview, menus, dependencies, logging summary
  - USER_GUIDE.md ............... in-depth introduction of features and output interpretation
- The simulator behavior is unchanged by this document.
- Trial/permanent access control is handled separately by edition markers and
  runtime checks (see trial_guard.py).
- The companion documents (including references and attributions) can be accessed under the "Help" menu on the program GUI (i.e., user interface). 

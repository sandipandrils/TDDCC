# TD-DCC — Time-Dependent Dynamic Cross-Correlation Analysis

TD-DCC is a standalone desktop application for computing and visualizing **time-resolved dynamical cross-correlation matrices (DCCMs)** from molecular dynamics (MD) trajectories.

Unlike a conventional DCCM — which collapses an entire trajectory into a single static matrix — TD-DCC computes correlations over a **sliding window of frames** and tracks how each residue pair's correlation evolves across the simulation: whether it stays consistently positive, consistently negative, or fluctuates between the two.

---

## Features

- **Built-in compute engine** — load a trajectory + topology directly; matrices are computed in parallel across sliding windows using all available cores
- **Statistics dashboard** — mean correlation, standard deviation, and positive/negative occurrence probability for every residue pair
- **Interactive heatmaps** — filtered Positive / Negative / Combined correlation maps with pan, zoom, and save
- **Per-pair time series** — double-click any residue pair to inspect its correlation trajectory window-by-window
- **CSV export** — download filtered results for downstream analysis in Python, R, or a spreadsheet
- **No internet required** — fully local; no API keys or cloud services

---

## System Requirements

- Python 3.9 or later (3.10–3.12 recommended)
- Windows or Linux
- 8 GB RAM recommended for large proteins
- A graphical desktop session (Tkinter-based GUI; X11 forwarding supported for remote use)

---

## Installation

### 1. Install system dependencies

**Ubuntu / Debian**
```bash
sudo apt update
sudo apt install python3 python3-pip python3-venv python3-tk
```

**Fedora / RHEL**
```bash
sudo dnf install python3 python3-pip python3-tkinter
```

**Windows** — download from [python.org/downloads](https://python.org/downloads) and check *"Add python.exe to PATH"* during install. Tkinter is included automatically.

---

### 2. Create a virtual environment (recommended)

**Conda**
```bash
conda create -n TD-DCC_env python=3.12
conda activate TD-DCC_env
```

**venv**
```bash
python3 -m venv TD-DCC_env
source TD-DCC_env/bin/activate      # Linux
TD-DCC_env\Scripts\activate         # Windows
```

---

### 3. Install Python dependencies

```bash
pip install numpy pandas matplotlib
```

To use the built-in trajectory compute engine, also install MDAnalysis:

```bash
pip install MDAnalysis
```

---

### 4. Download TD-DCC

Clone this repository or download `TD-DCC_gui.py` directly. No further files are needed.

```bash
git clone https://github.com/sandipandrils/TDDCC.git
cd TDDCC
```

---

## Quick Start

```bash
python3 TD-DCC_gui.py
```

Then click **"Launch Analysis Dashboard"** to open the main workspace.

### Workflow A — From an MD trajectory

1. Browse to your **trajectory** file (`.xtc`, `.dcd`, `.trr`, …)
2. Browse to your **topology** file (`.pdb`, `.tpr`, `.gro`, …)
3. Set **Window**, **Step**, and **Cores**, then click **COMPUTE DCC**
4. Matrices are written to the output directory automatically

### Workflow B — From existing correlation matrices

1. Point **Input Data** at a folder of pre-computed `.txt` matrix files
2. Set **Residue Limits** and **Correlation Filters**
3. Click **RUN ANALYSIS** → **View Correlation Dashboard**

> Double-click any cell in the heatmap to open the per-pair time series inspector.

---

## Output Files

| File | Description |
|---|---|
| `corr_dcc_<frame>.txt` | Plain-text N×N correlation matrix for one sliding window |
| `corr_dcc_<frame>.png` | Heatmap image of the same window (blue–white–red colormap) |
| `*_positive.csv` / `*_negative.csv` / `*_combined.csv` | Filtered residue-pair statistics (mean, SD, occurrence probability) |
| Time-series PNG / CSV | Per-pair correlation trajectory exported from the inspector |

---

## Sample Data

The `data/trajectory/` folder contains the AAV2 500 ns MD trajectory split into 5 parts for GitHub compatibility. To reconstruct the full trajectory before use:

**With MDAnalysis (Python)**
```python
import MDAnalysis as mda
u = mda.Universe("topology.pdb", [
    "data/trajectory/traj_AAV2_500ns_part1of5.xtc",
    "data/trajectory/traj_AAV2_500ns_part2of5.xtc",
    "data/trajectory/traj_AAV2_500ns_part3of5.xtc",
    "data/trajectory/traj_AAV2_500ns_part4of5.xtc",
    "data/trajectory/traj_AAV2_500ns_part5of5.xtc"
])
```

**With GROMACS**
```bash
gmx trjcat -f data/trajectory/traj_AAV2_500ns_part*of5.xtc -o full_traj.xtc -cat
```

---

## Documentation

Full installation instructions, interface tour, workflow details, and troubleshooting are in the [User Manual](TDDCC_User_Manual.md).

---

## Troubleshooting

| Symptom | Fix |
|---|---|
| `No module named 'tkinter'` | `sudo apt install python3-tk` (Linux) |
| COMPUTE DCC fails / MDAnalysis unavailable | `pip install MDAnalysis` |
| "No .txt files found" on Run Analysis | Point Input Data at the correct matrix output folder |
| Buttons cut off at bottom of screen | Scroll the left control panel downward |

See the [User Manual → Troubleshooting](TDDCC_User_Manual.md#8-troubleshooting) section for the full list.

---

## Citation

If you use TD-DCC in your research, please cite:

> *manuscript details to be added upon publication*

---

## License

*License details to be added.*

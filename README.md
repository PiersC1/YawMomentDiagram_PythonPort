# TLDR: Download/Clone, run, set up the sweep, run it, output saves to a folder in the current directory.

# Yaw Moment Diagram (YMD) Analysis Tool

This repository provides a comprehensive suite of Python tools to simulate, analyze, and visualize Yaw Moment Diagrams (YMDs) for a vehicle based on Pacejka Magic Formula tire models (Pending the NN Model) and configurable vehicle parameters. 

It includes interactive GUIs for editing parameters, performing single and double parameter sweeps, and extracting key performance indicators (KPIs) like Grip Limit, Limit Balance, Control, and Stability.

## Key Features

- **Base YMD Generation**: Simulate and visualize a vehicle's Yaw Moment vs. Lateral Acceleration for varying slip angles ($\beta$), steering angles ($\delta$), and slip ratios.
- **Parameter Sweeps**: Run single and double parameter sweeps over any vehicle parameter to see how changes affect vehicle handling.
- **Overlaying YMDs**: Generate "1D YMD Overlays" to visually compare the entire YMD envelope across different parameter values on the same plot.
- **KPI Extraction**: Automatically extract handling metrics directly from the YMD:
  - Grip Limit (Acceleration)
  - Limit Balance (Yaw Moment)
  - Control
  - Stability
- **Interactive Configuration GUI**: Load, edit, and save YAML configurations dynamically.
- **Saving**: All graphs and their corresponding config YAML are saved in a timestamped `Results_` folder.

## Project Structure

- **`YMD_KPIandSweeps.py`**: The primary entry point. Launches the advanced GUI (`param_gui_sweep.py`) to edit parameters and configure 1D/2D sweeps, then runs the physics simulation and saves plots and data to timestamped `Results_` folders.
- **`YMD_main.py`**: A simpler entry point for generating a single base YMD. Uses a basic parameter editor (`param_gui.py`).
- **`param_gui_sweep.py`** & **`param_gui.py`**: Tkinter-based interactive interfaces. The `sweep` version supports loading custom `.yaml` files, setting up parameter sweeps, and auto-populating sweep bounds.
- **`base_params_SCR26.yaml`**: The default vehicle configuration file. It contains mass parameters, suspension geometry, aero parameters, and more.
- **`magicformula.py`**: Implements a standard Pacejka Magic Formula tire model. Can use default generic coefficients or load specific `.mat` tire data.
- **`ackermann_solver.py`**: Calculates the precise inner and outer steering angles based on wheel track, wheelbase, and Ackermann percentage.
- **`wrapper.py`**: Object-oriented helper classes for advanced Matplotlib plotting used across the repository.

## Installation / Prerequisites

This project requires Python 3. You can install the required packages using `pip`:

```bash
pip install numpy scipy matplotlib pyyaml pandas
```

*Note: `tkinter` is used for the GUI and is typically included with standard Python installations. However, on some Linux distributions, you may need to install it separately (e.g., `sudo apt-get install python3-tk`).*

## Usage Details

### 1. Generating Sweeps & YMDs (Main Tool)

To launch the full suite with sweep capabilities:
```bash
python YMD_KPIandSweeps.py
``` 
or
```bash
python3 YMD_KPIandSweeps.py
```
or similarly launching from an IDE.

This will open the **YMD Parameter & Sweep Editor**:
1. You can adjust physical parameters in the respective tabs.
2. If you have a different configuration file (defaults to SCR26 base config), click **"Load different YAML..."** to import it. The UI will instantly reflect the loaded values.
3. Switch to the **1D Sweep** or **2D Sweep** tabs to select parameters to sweep over. You'll notice the start and end values auto-populate to ±20% (I know the numbers aren't pretty but it was the quickest solution that made sense to me) of the currently loaded parameter value.
4. Select which KPIs to save graphs for.
5. Click one of the "Run" buttons at the bottom corresponding to the desired output.

**Outputs:**
Results run from this script are saved automatically inside a newly created folder (e.g., `Results_KPI_Sweep_20260322_120000/`) in the current directory. The folder will contain:
- Configuration backups (`sweep_config.yaml` or `ymd_config.yaml`)
- KPI values logged in `yaml` format
- Generated Plots (`.png`) for YMD envelopes and KPI behavior across sweeps.

### 2. Generating a Basic YMD

If you just want a quick, singular plot without sweep options:
```bash
python YMD_main.py
```
This uses `param_gui.py` to let you tweak values and closes to reveal a Matplotlib interactive window with your YMD. (Note: This script is not as well developed as the sweep script and is only included for completeness, it doesn't even run faster than the updated version so just use that one).

### Configuration Files

By default, scripts will use `base_params_SCR26.yaml`. This file stores structural dictionaries defining everything from roll center height to anti-roll bar stiffness.

If you load past sweeps (e.g., `sweep_config.yaml`) back into the GUI, it will cleanly extract the base configuration from that past run, allowing you to iterate on a previous experimental setup.
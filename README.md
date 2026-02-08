<p align="center">
  <img width="150" src="./Images/KathleenWang_for_ezTrack.png">
</p>

# Behavior Tracking with ezTrack

This page hosts iPython files that can be used to track the location, motion, and freezing of an animal. For the sake of clarity, these processes are described as two modules: one for tracking an animal's location; the other for the analysis of freezing. **If you are unfamiliar with how to use iPython/Jupyter Notebook, please see [Getting Started](https://github.com/DeniseCaiLab/GettingStarted)**.

![Examples](../master/Images/Examples.gif)

# Please cite ezTrack if you use it in your research:

Pennington ZT, Dong Z, Feng Y, Vetere LM, Page-Harley L, Shuman T, Cai DJ (2019). ezTrack: An open-source video analysis pipeline for the investigation of animal behavior. Scientific Reports: 9(1): 19979

# Installation and Package Requirements

Installation is the same as described in the original ezTrack wiki, using a Conda environment

1. Download and install Miniconda/Conda with the latest Python version.

2. Create the `ezTrack` Conda environment.

- On macOS/Linux, open Terminal. On Windows, open **Anaconda Prompt** (or PowerShell).
- **Recommended (one-time):** Use the faster libmamba solver so environment solving doesn’t hang. Run once:

  ```bash
  conda install -n base conda-libmamba-solver
  conda config --set solver libmamba
  ```

- Then create the environment with one of the following.

  **macOS/Linux (multi-line):**

  ```bash
  conda create -y -n ezTrack -c conda-forge \
    python=3.9.16 jupyter=1.0.0 numpy=1.24 scipy=1.11.1 pandas=2.0.3 \
    opencv=4.7.0 holoviews=1.15.0 bokeh=2.4.0 pyviz_comms=2.1 \
    jinja2=3.1.2 scikit-learn=1.3.0 matplotlib=3.7.2 tqdm=4.65.0
  ```

  **Windows (PowerShell/CMD — use one line; backslash does not continue lines):**

  ```powershell
  conda create -y -n ezTrack -c conda-forge python=3.9.16 jupyter=1.0.0 numpy=1.24 scipy=1.11.1 pandas=2.0.3 opencv=4.7.0 holoviews=1.15.0 bokeh=2.4.0 pyviz_comms=2.1 jinja2=3.1.2 scikit-learn=1.3.0 matplotlib=3.7.2 tqdm=4.65.0
  ```

# Source control and notebooks

Notebooks (`.ipynb`) store both **code** (what you want in git) and **outputs** (plots, stdout, execution counts). Git can't ignore “only the output” of a file, so this repo strips outputs when you commit. That keeps history small, diffs readable, and avoids merge conflicts from re-running cells.

**One-time setup** (from the repo root, with the `ezTrack` env active):

```bash
pip install nbstripout
nbstripout --install
```

This configures a git filter so that when you `git add` a notebook, its outputs are removed before the file is staged. Your local copy still has outputs until you re-run the notebook.

**Workflow:**

- Edit and run notebooks as usual; outputs stay in your working copy.
- On `git add` / `git commit`, notebook outputs are stripped automatically. Only code and markdown are committed.
- After `git pull` or a fresh clone, run the notebook (e.g. “Run All”) to regenerate outputs.

**Optional:** For clearer notebook diffs and merges, install [nbdime](https://nbdime.readthedocs.io/) and configure it as your diff/merge tool; it can show cell-level changes and optionally hide output in diffs.

# New Feature Alerts:

- 02/08/2026: Location tracking **processing revised**: frame–reference difference is thresholded to a **binary mask** with optional preprocessing (Gaussian blur, CLAHE). Morphological **open/close** and wire removal run on the mask; **connected-component blobs** are filtered by **min/max blob area** and the position is the **centroid of the selected blob** (largest, or nearest to prior when `use_window` is on)—replacing raw intensity-weighted center of mass for more robust tracking. Optional **live preview** and **video export** during analysis.
- 04/11/2021: ezTrack now has **algorithm for removing wires** in the location tracking module.
- 07/20/2020: ezTrack now supports **spatial downsampling** of videos! You can reduce the resolution of the video to greatly speed processing. Processing high-definition videos on older laptops/desktops can be slow, but by downsampling, processing speeds are much faster.
- 07/19/2020: Location tracking module now allows user to **manually define frame numbers to be used when selecting reference**. This is useful if baseline portion of video without animal will be used for reference, and resolves issue when alternative video being used for reference is a different length than the video being processed.
- 06/16/2020: Location tracking module **now allows user to define regions of frame that they would like excluded from the analysis**. This is useful in situations where an extraneous object enters into periphery, or even center, of the field of view.

# Location Tracking Module

The location tracking module allows for the analysis of a single animal's location on a frame by frame basis. In addition to providing the user the with the ability to crop the portion of the video frame in which the animal will be, it also allows the user to specify regions of interest (e.g. left and right sides) and provides tools to quantify the time spent in each region, as well as distance travelled.  
![schematic_lt](../master/Images/LocationTracking_Schematic.png)

> [!TIP]
> Due to the way the COM is calculated it is important that the starting frames of the video include the object you are wishing to track, especially when the contrast between shadows and your target object is minimal.

# Freeze Analysis Module

The freeze analysis module allows the user to automatically score an animal's motion and freezing while in a conditioning chamber. It was designed with side-view recording in mind, and with the intention of being able to crop the top of a video frame to remove the influence of fiberoptic/miniscope cables. In the case where no cables are to be used, recording should be capable from above the animal.  
![schematic_fz](../master/Images/FreezeAnalysis_Schematic.png)

# License

This project is licensed under GNU GPLv3.

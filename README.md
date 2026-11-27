# bionic-arm-emg-gestures

## Overview
EMG signal processing and gesture classification for a bionic arm project. It includes a Bluetooth data logger for a MyoWare 2.0 EMG sensor, a filtering pipeline tested on the public GRABMyo dataset, and baseline Random Forest and SVM gesture classifiers built on time-domain features.

## Methods
- `emg_collection_bionicarm_v1.py`: connects to the sensor over Bluetooth LE (`bleak`), records one byte per notification (0–255) with a timestamp, and saves a CSV when stopped.
- `bionic_arm_week1.py`
  - Downloads GRABMyo session 1 records.
  - Filtering: 4th-order Butterworth bandpass (20–450 Hz), 60 Hz notch (Q = 30), full-wave rectification, RMS envelope (200 ms window, 50% hop).
  - Plots envelopes for gestures 1–4 and computes simple envelope features (mean, std, max, min, slope).
- `bionic_arm_week2.py`
  - Bandpass and notch filtering on every channel.
  - Six features per channel per 200 ms window with 50% overlap: MAV, RMS, ZC, WL, SSC and VAR.
  - Data: participant 1, gestures 1–4, trials 1–2. Stratified 80/20 split, with the scaler fit on the training set only.
  - Models: Random Forest (100 trees) and SVM (RBF, C = 10). Outputs a confusion matrix and the top 20 Random Forest feature importances.
  - Ends with planning notes for a model that accounts for electrode placement.

Results:  The code prints accuracy at run time but stores no results. All windows come from one participant and are split randomly, so test accuracy is likely optimistic.

## Tools
Python, NumPy, SciPy, pandas, scikit-learn, Matplotlib, wfdb, bleak (Bluetooth LE), Arduino + MyoWare 2.0 sensor, Google Colab.

## Status
Complete.

**Data:** not included. The scripts download GRABMyo v1.1.0 from PhysioNet. TODO: confirm the GRABMyo citation and license text. Week 1 also clones the third-party repository `kaviles22/EMG_SignalClassification`.

**Notes:** The `.py` files are exports of Google Colab notebooks. Lines starting with `!` (and the bare `pip install` line, where present) are notebook shell commands, and `drive.mount(...)` calls are Colab-only. Run the files cell by cell in a notebook, not as plain scripts.

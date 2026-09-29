# Philips2MRD
Converter(s) for Philips raw data to ISMRMRD format.

Changes in this branch are created by Riaz Hussain, PhD.

## rh-dev branch
- Default trajectory `.sin` always used (no prompt); fixed renamed Duke `.sin` path
- Stale `_noBonus.h5` removed before rewrite (no duplicate acqs)
- Proton: own `.sin` and delay, scaled by acquired-matrix ratio, trimmed to |k|<=0.5
- Calibration: fixed `ext_traj` bug
- Duke: own `.sin`, Halton order, gas = dyn0; `-i/--institution` option
- Gas/dissolved k0 sanity check; dwell fallback for old `.sin`
- `requirements.txt` added

## Setup
```
conda create -n philips2mrd python=3.12
conda activate philips2mrd
pip install -r requirements.txt
```

## Usage

Used to convert Philips scanner acquired Xe gas exchange (Dixon), calibration, and Mask (proton) scans to XeCTC compliant MRD files, which can then be analyzed with (https://github.com/Xe-MRI-CTC/xenon-gas-exchange-consortium). Or my branch (rh-dev) for some extra features.

To run:
```
python Scripts/XeGasExchange2XeCTCMRD.py -d <raw_XXX.data> -r <YYYYMMDD_HHMMSS_scan.raw> [-i CCHMC|Duke|Polarean]
```
- Run without `-d`/`-r` to pick files from dialog.
- Keep `.data/.list` and `.raw/.lab/.sin` in the same folder; `.raw` name must start with scan date (YYYYMMDD).
- Output is named after the folder: `<folder>_dixon.h5`, `<folder>_proton.h5`, `<folder>_calibration.h5`.
- `-i` (default CCHMC): Duke is also detected from scan name.
  - CCHMC: default trajectory `.sin` from `resources/`.
  - Duke: scan's own `.sin`.
  - Polarean: Polarean `.sin` from `resources/`.
- `-t <file.sin>`: optional, override trajectory `.sin`.

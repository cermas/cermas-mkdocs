# CASTEP Runner

You find our python package to run automated scans in castep using the wraper castep-mpi on ubuntu plataforms.
[here](https://github.com/cermas/castep_python_runner)


# CASTEP Runner — User Guide

---

## Introduction

`CASTEP Runner` is a modular Python wrapper that automates launching `castep-mpi` calculations. It helps organize runs in dedicated folders, automatically edits CASTEP input files, and supports parameter scans.

**Highlights:**

* Edits `cut_off_energy` in `.param` files.
* Edits `kpoint_mp_grid` in `.cell` files.
* Stages all relevant input files into a clean run directory.
* Executes `castep-mpi <np> <seedname>` inside that directory.
* Supports **single runs** and **scans** (cutoff or k-grid).
* Provides `--dry-run` mode for setup without execution.

---

## Installation

Requirements:

* Python 3.8+
* `castep-mpi` in your `$PATH`

Clone or copy the package structure:

```
project/
├─ run_castep.py
└─ castep_runner/
   ├─ __init__.py
   ├─ cli.py
   ├─ io_utils.py
   ├─ param_edit.py
   ├─ cell_edit.py
   ├─ runner.py
   └─ constants.py
```

Make the entry point executable:

```bash
chmod +x run_castep.py
```

---

## Usage

### Single cutoff run

```bash
python run_castep.py <NP> <SEEDNAME> --cutoff <INT>
```

* Creates `cutoffs/<seedname>_<cutoff>/`
* Stages inputs
* Ensures `cut_off_energy : <cutoff> eV` in `.param`
* Runs CAStep

### Cutoff scan

```bash
python run_castep.py <NP> <SEEDNAME> --cutoff-scan START END STEP
```

* Generates subdirectories for each cutoff value.
* **Inclusion rule:** always include `START`; include `END` only if reached exactly by stepping.

### Single k-point grid

```bash
python run_castep.py <NP> <SEEDNAME> --kgrid NX NY NZ
```

* Creates `kgrids/<seedname>_<NX>x<NY>x<NZ>/`
* Edits `.cell` with `kpoint_mp_grid NX NY NZ`
* Runs CAStep

### K-grid scan

```bash
python run_castep.py <NP> <SEEDNAME> --kgrid-scan S1 E1 T1  S2 E2 T2  S3 E3 T3
```

* **First triple** `(S1,E1,T1)`: starting grid.
* **Second triple** `(S2,E2,T2)`: ending grid.
* **Third triple** `(S3,E3,T3)`: step increments.
* All three axes advance together. Non-varying axes should have step `0`.

**Examples:**

* Vary only Nx:

  ```bash
  python run_castep.py 4 SiO2 --kgrid-scan 2 2 2  6 2 2  2 0 0
  # → (2 2 2), (4 2 2), (6 2 2)
  ```
* Vary all axes equally:

  ```bash
  python run_castep.py 4 SiO2 --kgrid-scan 2 2 2  8 8 8  2 2 2
  # → (2 2 2), (4 4 4), (6 6 6), (8 8 8)
  ```

**Inclusion rule:** start is always included; end is included only if exactly reached.

### Dry run

```bash
python run_castep.py 4 SiO2 --cutoff 700 --dry-run
```

Prepares the directory and edits inputs but does not execute CAStep.

---

## Input Staging

* Copies `.cell`, `.param`, `.usp`, and other relevant files.
* Copies any `seedname.*` file as a catch-all.
* Existing files in run directories are not overwritten.

---

## Editing Behavior

* **.param:** ensures a line `cut_off_energy : <value> eV` exists and updates it.
* **.cell:** ensures a line `kpoint_mp_grid <nx> <ny> <nz>` exists and updates it.

---

## Output Organization

* **Cutoff runs:** `cutoffs/<seedname>_<cutoff>/`
* **K-grid runs:** `kgrids/<seedname>_<nx>x<ny>x<nz>/`

Each folder contains both inputs and outputs, ensuring reproducibility.

---

## Exit Codes

* `0` — success
* `127` — `castep-mpi` not found
* `1` — missing required input file (`.param` or `.cell`)
* `2` — invalid scan arguments
* Non-zero from CAStep — propagated upwards

---

## Extending

* **Parallel scans:** add `--jobs N` and use multiprocessing.
* **Additional parameters:** implement in `param_edit.py` or `cell_edit.py`.
* **Scheduler integration:** replace direct run with SLURM/LSF submission.

---

## Programmatic API

```python
from castep_runner.io_utils import ensure_run_dir, stage_inputs
from castep_runner.param_edit import set_cutoff_in_param
from castep_runner.cell_edit import set_kgrid_in_cell
from castep_runner.runner import run_castep

run_dir = ensure_run_dir("SiO2", 700)
stage_inputs("SiO2", run_dir)
set_cutoff_in_param(run_dir/"SiO2.param", 700)
set_kgrid_in_cell(run_dir/"SiO2.cell", 4, 4, 4)
rc = run_castep(4, "SiO2", run_dir)
```

---

## Maintainer Notes

* Keep CLI logic in `cli.py`.
* Keep editing isolated in `param_edit.py` and `cell_edit.py`.
* Add tests around scan behavior (cutoff and k-grid) to ensure reproducibility.


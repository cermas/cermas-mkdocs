# Installation
## Overview
This document describes how to install SIMPSON 4.2.1 on Ubuntu, and how to fix a common Tcl version mismatch error.

The process was tested successfully on:

- Ubuntu 24.04 + AMD processor (with optimized BLAS/LAPACK/FFTW for AMD)

- Ubuntu 24.04 + Intel i9 processor (with optimized BLAS/LAPACK/FFTW for Intel)

Both setups also included MPI (mpirun) for parallel execution.
## 1. Installation
After downloading SIMPSON 4.2.1, run the installer script:
```bash
sudo ./install.sh
```
This installs the simpson wrapper into /usr/local/bin and the libraries into /usr/share/simpson

If you try
```bash
simpson test.in
```
you going to face the following error:
```bash
SIMPSON is unable to initialize Tcl interpreter. Is init.tcl on your path? Error: Can't find a usable init.tcl in the following directories: /usr/share/simpson/tcl8.6 /usr/share/tcltk/tcl8.6 ./lib/tcl8.6 ./lib/tcl8.6 ./library ./library ./tcl8.6.5/library ./tcl8.6.5/library /usr/share/tcltk/tcl8.6/init.tcl: version conflict for package "Tcl": have 8.6.5, need exactly 8.6.14 version conflict for package "Tcl": have 8.6.5, need exactly 8.6.14 while executing "package require -exact Tcl 8.6.14" (file "/usr/share/tcltk/tcl8.6/init.tcl" line 19) invoked from within "source /usr/share/tcltk/tcl8.6/init.tcl" ("uplevel" body line 1) invoked from within "uplevel #0 [list source $tclfile]"
```
The cause is SIMPSON installer ships with an outdated Tcl 8.6.5 but the SIMPSON 4.2.1 binary is built against Tcl 8.6.14. This mismath prevents startups.

### Fixing the tcl issue
Instead of using the bundled Tcl, we point SIMPSON to Ubuntu’s system Tcl (already at 8.6.14). Edit the following wrapper script:
```bash
sudo nano /usr/local/bin/simpson
```
Change only these two lines:
```bash
export TCL_LIBRARY=/usr/share/simpson/tcl8.6
export LD_LIBRARY_PATH=/usr/share/simpson
``` 
to 
```bash
export TCL_LIBRARY=/usr/share/tcltk/tcl8.6
export LD_LIBRARY_PATH=/usr/lib/x86_64-linux-gnu
```
save and exit.
## Verification
Run now the same command:
```bash
simpson test.in
```
## Parallel execution
If you have installed the packages for parallel execution (you can refer the DFT documentation in this website to install them). You can run
```bash
mpirun -np 8 simpson test.in
```
The flag ``np`` is used to account for the number of total processor. If you have a 4 machines with 2 cores, then your np should be 4x2, which is 8 in the example above.
## Notes on Hardware
-  On AMD-based Ubuntu, SIMPSON runs correctly with AMD-optimized math libraries (BLAS/LAPACK/FFTW).

- On Intel i9-based Ubuntu, SIMPSON also runs correctly with Intel-optimized libraries.

Both environments had MPI (mpirun) installed for distributed simulations, and no MPI-related issues occurred.
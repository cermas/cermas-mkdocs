# Installation CASTEP - MKL
## CASTEP 25.12 + Intel MKL + OpenMPI on Ubuntu 24.04

This guide explains how to install and run **CASTEP 25.12** with **Intel MKL** and **OpenMPI** on Ubuntu 24.04.

---

## System Information

- **CPU**: `Intel Core i9` (24 cores / 32 threads)
- **RAM**: `128 GB`
- **OS**: `Ubuntu 24.04 LTS`
- **Compilers**: `GNU Fortran 13.x`
- **MPI**: `OpenMPI 4.1.x`
- **Math library**: `Intel MKL` (installed via `apt`)

---

## Prerequisites

Install required packages:

```bash
sudo apt update
sudo apt install -y build-essential gfortran cmake git \
    libopenmpi-dev openmpi-bin \
    libfftw3-dev python3 python3-pip gawk intel-mkl
```

## Locating MKL Libraries

MKL libraries are installed in `/lib/x86_64-linux-gnu/` and headers in `/usr/include/mkl`.

Check:
```bash
ldconfig -p | grep mkl
ls /usr/include/mkl
```

## Environment Setup
Add to your `~/.bashrc`:
```bash
# === Intel MKL ===
export MKL_LIB=/lib/x86_64-linux-gnu
export MKL_INC=/usr/include/mkl
export INCLUDE=$MKL_INC
export LD_LIBRARY_PATH=$MKL_LIB:$LD_LIBRARY_PATH

# Avoid MPI + MKL thread oversubscription
export MKL_INTERFACE_LAYER=LP64
export MKL_THREADING_LAYER=GNU
export OMP_NUM_THREADS=1

# === CASTEP binaries ===
export PATH=$HOME/CASTEP/CASTEP-25.12/bin/linux_x86_64_gfortran10--mpi:$PATH
```

## Building CASTEP
Unpack the source:
```bash
tar -zxf CASTEP-25.12.tar.gz
cd CASTEP-25.12
```
Clean previous build 

```bash
rm -rf obj/linux_x86_64_gfortran10--mpi
```
## MPI build with MKL
First run make

!!! warning "DL_MG=None"
    In this setup, we were not able install dl_mg module. We have tried several times and the following error kept being thrown out during make statement.Error:
    ```
    /usr/bin/ld: /home/numsim/CASTEP/CASTEP-25.12/obj/linux_x86_64_gfortran10--mpi/build/../../../Source/Functional/multigrid_dlmg.f90:444:(.text+0x146db): undefined reference to __dl_mg_MOD_dl_mg_error_string' collect2: error: ld returned 1 exit status make[1]: *** [Makefile:220: ../castep.mpi] Error 1 rm f90wrap_stub.anc make[1]: Leaving directory '/home/numsim/CASTEP/CASTEP-25.12/obj/linux_x86_64_gfortran10--mpi/build' make: *** [make.inc:236: castep] Error 2
    ```
As a solution we have chosen the following command:
```bash
make -j$(nproc) COMMS_ARCH=mpi DL_MG=none \
  MATHLIBS=mkl MATHLIBDIR="$MKL_LIB" \
  FFT=mkl FFTLIBDIR="$MKL_LIB" \
  EXTRA_LIBS="-Wl,--start-group -lmkl_intel_lp64 -lmkl_gnu_thread -lmkl_core -Wl,--end-group -lsymspg -lgomp -lpthread -lm -ldl" \
  LDFLAGS="-L$MKL_LIB -Wl,--no-as-needed -Wl,-rpath,$MKL_LIB" \
  BUILD=fast TARGETCPU=host
```
then install:
```bash
make COMMS_ARCH=mpi install
```

## Verifying installation

Check that the binary links against MKL and spglib:
```bash
ldd bin/linux_x86_64_gfortran10--mpi/castep.mpi | grep -E "mkl|symspg"
```
Run version header check:
```bash
mpirun -np 4 bin/linux_x86_64_gfortran10--mpi/castep.mpi --version
```
Expected: MKL and OpenMPI are listed.

## Wrapper Script
To run only commadn castep-mpi as default
```bash
sudo bash -lc 'cat > /usr/local/bin/castep-mpi << "EOF"
#!/bin/bash
# Usage: castep-mpi <nprocs> <seed> [args...]
NPROCS="$1"; SEED="$2"; shift 2
mpirun -np "$NPROCS" --bind-to core "$HOME/CASTEP/CASTEP-25.12/bin/linux_x86_64_gfortran10--mpi/castep.mpi" "$SEED" "$@"
EOF
chmod +x /usr/local/bin/castep-mpi'
```
Now you run jobs with:
```bash
castep-mpi 24 si
```

## Troubleshoting
MPI_ABORT error at start: missing pseudopotential → check SPECIES_POT block or PSPOT_DIR.<br/>

Falls back to generic BLAS (-llapack -lblas): ensure MKL paths are set and passed to make.<br/>

Undefined DL_MG symbols: either disable (DL_MG=none) or build DL_MG (DL_MG=compile) if available in your source<br/>

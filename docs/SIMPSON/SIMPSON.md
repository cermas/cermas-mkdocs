# SIMPSON Overview
## What is SIMPSON?

SIMPSON (Simulation Program for Solid-State NMR) is a powerful open-source program for simulating nuclear magnetic resonance (NMR) experiments, especially in the solid state.

It was originally developed by Thomas Vosegaard and collaborators, and is widely used in research groups worldwide for designing, analyzing, and understanding complex NMR experiments.

At its core, SIMPSON allows users to:

- Define spin systems and Hamiltonians

- Simulate pulse sequences (1D, 2D, MAS, multi-pulse)

- Include powder averaging and relaxation models

- Compare simulated spectra with experimental results

## Why Use SIMPSON?

- Flexibility: Supports a broad range of NMR experiments (solid-state and liquid-state).

- Accuracy: Handles both simple and highly complex spin systems.

- Extendability: Uses Tcl scripting for defining simulations, making it customizable.

- Performance: Supports MPI (via mpirun) and optimized math libraries (BLAS/LAPACK/FFTW).

SIMPSON is particularly useful when:

Designing new experiments before performing them on the spectrometer.

Understanding the effects of anisotropic interactions (CSA, dipolar, quadrupolar).

Benchmarking experimental data against theoretical predictions.

In this page, you also will find how to install SIMPSON program in your linux environment.
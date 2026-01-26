# Hahn Echo and Echo trains (Theory-First Lecture Note)

!!! abstract "Scope and philosophy"
    This note explains **Hahn echo** and **CPMG** using a theory-first approach aimed at simulation and quantitative reasoning (rather than spectrometer operation). We work from the density-operator viewpoint and keep the **minimum Hamiltonian**. <br/>
    We omit interactions (dipolar couplings, CSA, quadrupolar terms, finite pulse effects, relaxation), we **state explicitly** what is being omitted and why.

---

## 1. What is being measured in NMR?

### 1.1 Inductive detection is linear in transverse magnetization
We start by minimal look at the experimental setup. The RF (radio-frequency) coil detects a voltage proportional to the **time derivative** of the sample’s transverse magnetization. In standard NMR theory (and in most simulation frameworks), this is captured by the expectation value of transverse spin operators:<br/>

$$\begin{equation}
s(t)\;\propto\;\langle I_x\rangle(t) + i\langle I_y\rangle(t)
\;\equiv\;\langle I_+\rangle(t),
\qquad I_+=I_x+iI_y.
\end{equation}$$

In the density-operator language:

$$\begin{equation}
\langle I_\alpha\rangle(t)=\mathrm{Tr}\!\left[I_\alpha\,\rho(t)\right],\qquad \alpha\in\{x,y,+\}.
\end{equation}$$

**Key point:** only the part of \(\rho(t)\) proportional to \(I_x\) and/or \(I_y\) contributes directly to the observed signal. The experimental aparatus is set so that you can detect signal of one of those quantities. At high field and high temperature (the usual approximation in solid-state NMR theory), we take the following approximation

$$\begin{equation}
\rho_{\mathrm{eq}} \propto I_z.
\end{equation}$$

This captures the fact that equilibrium magnetization is along the static magnetic field (laboratory \(z\)-axis). Throughout this lecture we always assume these two ingredients as a starting point. 

---

## 2. The minimal model for echoes: static offsets

### 2.1 Hamiltonian
To explain echoes, we use a single-spin Hamiltonian with a static frequency offset:

$$\begin{equation}
H = \Delta\omega\, I_z.
\end{equation}$$

Here \(\Delta\omega\) represents the difference between a given spin packet's precession frequency and the rotating frame reference (due to \(B_0\) inhomogeneity and/or chemical shift dispersion). This model excludes any spin interaction inside the sample.

From this hamiltonian we can define the free propagator for a time interval \(\tau\)

$$\begin{equation}
U(\tau)=e^{-iH\tau}=e^{-i\Delta\omega I_z\tau}.
\end{equation}$$

Conjugation by \(U(\tau)\) rotates transverse operators in the \(xy\)-plane. If we define \(\phi=\Delta\omega\tau\) then:

$$\begin{equation}
U(\tau)\,I_x\,U^\dagger(\tau)=I_x\cos\phi + I_y\sin\phi,
\end{equation}$$

$$\begin{equation}
U(\tau)\,I_y\,U^\dagger(\tau)=I_y\cos\phi - I_x\sin\phi,
\end{equation}$$

$$\begin{equation}
U(\tau)\,I_z\,U^\dagger(\tau)=I_z.
\end{equation}$$

!!! note "About notation (important)"
    The operators \(I_x, I_y, I_z\) on the **right-hand side** are the fixed basis operators (time-zero operators).  
    The time dependence appears only in the coefficients \(\cos(\Delta\omega\tau)\) and \(\sin(\Delta\omega\tau)\).  
    This avoids the ambiguity of writing \(I_x(t)\) and then also using \(I_x\) on the right.

---

The relations above are found by the following steps

$$\begin{equation}
I_x(t) = U(\tau)\,I_x\,U^\dagger(\tau) = e^{i\Delta \omega \tau I_z}I_x e^{-i\Delta \omega \tau I_z}
\end{equation}$$

$$\begin{equation}
\frac{d}{dt}I_x(t) = i\Delta \omega     e^{i\Delta \omega \tau I_z}[I_z,I_x] e^{-i\Delta \omega \tau I_z}
\end{equation}$$

since \([I_z,I_x]=i I_y\) then

$$\begin{equation}
\frac{d}{dt}I_x(t) = -\Delta \omega     I_y(t)
\end{equation}$$

Similarly,

$$\begin{equation}
I_y(t) = \Delta \omega I_x(t)
\end{equation}$$

Solving the system of equation, we have then

$$\begin{equation}
I_x(t) = I_x\cos\Delta \omega t + I_y\sin\Delta \omega t
\end{equation}$$

$$\begin{equation}
I_y(t) = I_x\cos\Delta \omega t - I_y\sin\Delta \omega t
\end{equation}$$


## 3. The Hahn echo (spin echo)

### 3.1 The sequence
Now that we know how to compute the evolution of our spin operators we should introduce the simplest case of a spin echo. The Hahn echo sequence is

$$\begin{equation}
90^\circ_y\;-\;\tau\;-\;180^\circ_x\;-\;\tau\;-\;\text{detect}.
\end{equation}$$

It produces a refocused signal (an echo) at time \(t=2\tau\) after the first pulse.

### 3.2 Pulse action as rotations
A hard pulse is modeled as an instantaneous rotation, experimentally, that is not always the case, but for theoretical purpose we can represent like this

$$\begin{equation}
R_\alpha(\theta)=e^{-i\theta I_\alpha},\qquad \alpha\in\{x,y,z\}.
\end{equation}$$

It transforms operators by conjugation: \(A\mapsto R_\alpha(\theta)\,A\,R_\alpha^\dagger(\theta)\).

The pulse identities used below are: <br/>
- \(90^\circ_y\): \(I_z \to I_x\).<br/>
- \(180^\circ_x\): \(I_x\to I_x\), \(I_y\to -I_y\), \(I_z\to -I_z\).<br/>

We omit finite pulse duration, RF inhomogeneity, and off-resonance during pulses for simplicity and these omissions shall be addressed later.
From that point of view, we can do a step-by-step derivation  (density operator evolution)

Start:

$$\begin{equation}
\rho_0 \propto I_z.
\end{equation}$$

**Step 1 — Apply \(90^\circ_y\) (create transverse magnetization)**

$$\begin{equation}
\rho_1 \propto R_y(\pi/2)\,I_z\,R_y^\dagger(\pi/2)=I_x.
\end{equation}$$

**Step 2 — Free evolution for \(\tau\) (dephasing)**

$$\begin{equation}
\rho_2 \propto U(\tau)\,I_x\,U^\dagger(\tau)=I_x\cos\phi + I_y\sin\phi,
\qquad \phi=\Delta\omega\tau.
\end{equation}$$

Interpretation: different isochromats have different \(\Delta\omega\), hence different \(\phi\), so the ensemble average of the transverse magnetization decays (FID).

**Step 3 — Apply \(180^\circ_x\) (invert transverse phase dispersion)**
Using \(I_x\to I_x\) and \(I_y\to -I_y\):

$$\begin{equation}
\rho_3 \propto I_x\cos\phi - I_y\sin\phi.
\end{equation}$$

**Step 4 — Free evolution for \(\tau\) (rephasing)**

Now propagate each term by another \(\tau\):

$$\begin{equation}
\rho_4 \propto U(\tau)\,\rho_3\,U^\dagger(\tau)
= \cos\phi\,U(\tau)I_xU^\dagger(\tau) - \sin\phi\,U(\tau)I_yU^\dagger(\tau).
\end{equation}$$

Insert the rotation identities:

$$\begin{equation}
\rho_4 \propto \cos\phi\,(I_x\cos\phi + I_y\sin\phi)\;-\;\sin\phi\,(I_y\cos\phi - I_x\sin\phi).
\end{equation}$$

Collect \(I_x\) and \(I_y\) terms:<br/>
- \(I_x\): \(\cos^2\phi + \sin^2\phi = 1\),<br/>
- \(I_y\): \(\cos\phi\sin\phi - \sin\phi\cos\phi = 0\).<br/>

Therefore:

$$\begin{equation}
\boxed{\rho_4 \propto I_x,}
\end{equation}$$

**independent of** \(\Delta\omega\).

---

### 3.4 Why this is an echo
At time \(t=2\tau\), *every* spin (every \(\Delta\omega\)) has been driven back to the same transverse operator \(I_x\). Hence the macroscopic transverse magnetization reappears, producing an echo signal. This is the central idea with echo train experiments.

!!! important "What Hahn echo refocuses (and what it does not)"
    - Hahn echo refocuses **static** offsets \(\Delta\omega\) (e.g., \(B_0\) inhomogeneity, static chemical shift dispersion).  
    - It does **not** refocus truly irreversible decoherence/relaxation processes. If these are present, the echo amplitude decays with a characteristic time scale closer to \(T_2\). This changes the signal in the experiment completely. We have also not cover interaction between spins such as dipole, quadrupole, chemical shift anisotropy. Even though, they tend to be present, under MAS condition, we have qualitatively similar behavior for solids.

---

Often in a experiment we tend to use repetition of the echo. One usual repetition was apply by Carr-Purcell-Meiboom-Gill (CPMG)<br/>
## 4. From Hahn echo to CPMG: echo trains
A canonical CPMG train is:

$$\begin{equation}
90^\circ_{y}\;-\;\tau\;-\;\big[180^\circ_{x}\;-\;2\tau\big]_{N}\;-\;\text{acquire echoes}.
\end{equation}$$

Echo maxima appear at \(t=2\tau,\,4\tau,\,6\tau,\dots\) after the first pulse (depending on the precise timing convention).

### 4.2 Why an echo train helps

Echo trains provide:<br/>
- **Sensitivity boost:** you can sum many echoes (or sample them efficiently).<br/>
- **Robust decay estimation:** the envelope of echo amplitudes can be fit to obtain an effective transverse decay constant (often related to \(T_2\) or an effective \(T_2\) in solids).<br/>
- **Mitigation of receiver dead time:** the early FID may be lost, but echoes occur later and can be observed cleanly.<br/>


---

## 5. Suggested exercises (for mastery)

1. **Hahn echo with explicit ensemble average:**  
   Assume a Gaussian distribution \(g(\Delta\omega)\) and compute the FID and the echo amplitude with and without the \(180^\circ\) pulse.

2. **Single-pulse error leakage:**  
   Starting from \(\rho\propto I_y\), apply \(R_x(\pi+\varepsilon)\) exactly (no small-angle approximation) and identify the stored \(I_z\) component.

---



!!! tip "What should be our next lecture?"
    Go to our github repository and upload your suggestion there. Any suggestion will be deeply appreciated.


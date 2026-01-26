# Lecture 1 – Basics of NMR: Classical and Quantum Pictures

> *“It's the job that's never started as takes longest to finish.”*
_J.R.R. Tolkien (Lord of the Rings)_

These notes introduce two complementary ways of thinking about NMR:

1. A **classical vector model**, in which we follow a macroscopic magnetization vector \( M \) in a magnetic field.
2. And a **quantum model**, in which we describe individual nuclear spins with wavefunctions and operators.

Both pictures describe two different physics that results into measuring macroscopically the same quantity, ie., magnetic signal from a sample.

---

## 1. Classical Vector Model of NMR

### 1.1 Net Magnetization from Many Spins

Real NMR signals come from many nuclei – typically on the order of Avogadro’s number. Each nucleus carries a magnetic moment vector \( \mu_i \). Summing them over the whole sample gives the **net magnetization** which can be written as:

$$\begin{equation}
M = \sum_{i} \mu_i
\end{equation}$$

- In **zero magnetic field**, the individual moments are randomly oriented, ie, their vector sum is (on average) zero.
- In a **static magnetic field** \( B_0 \), more spins align along the field direction than against it, ie, a small but nonzero net magnetization \( M \) appears.

> **Placeholder for figure:**  
> *[Fig. 1: Random spin orientations with \(B_0 = 0\) vs. slight alignment when \(B_0 \neq 0\).]*

Later, we’ll connect this classical \( M \) to underlying quantum populations. But let us not get ahead of ourselves.

---

### 1.2 Torque on Magnetization in a Static Field

Classically, a magnetic moment \( \mu \) in a field \( B \) experiences a **torque**

$$\begin{equation}
\tau = \mu \times B
\end{equation}$$

For the whole sample, we treat the macroscopic magnetization \( M \) as playing the role of a single big magnetic moment. Then

$$\begin{equation}
\tau = M \times B
\end{equation}$$

On the other hand, torque is also related to the **rate of change of angular momentum** \( J \):

$$\begin{equation}
\tau = \frac{dJ}{dt}
\end{equation}$$

For nuclear spins, the magnetic moment and angular momentum are proportional:

$$\begin{equation}
\mu = \gamma I
\end{equation}$$

for a single spin, where

- \( I \) = nuclear spin angular momentum,
- \( \gamma \) = magnetogyric ratio (a constant characteristic of each nucleus, e.g. \(^1\)H, \(^{13}\)C).

Extending this to the ensemble we have

$$\begin{equation}
M = \gamma J
\end{equation}$$

So

$$\begin{equation}
\tau 
= \frac{dJ}{dt} 
= \frac{1}{\gamma} \frac{dM}{dt}
\end{equation}$$

Equating this with the torque from the magnetic field:

$$\begin{equation}
\frac{1}{\gamma}\frac{dM}{dt} = M \times B
\end{equation}$$

or

$$\begin{equation}
\boxed{
\frac{dM}{dt} = \gamma\, M \times B
}
\end{equation}$$

This is the **equation of motion** for the magnetization vector.

---

### 1.3 Larmor Precession

Now we have to specialize to a **static, uniform magnetic field** along the \(z\)-axis which is typically the case we face in the experiments:

$$\begin{equation}
B_0 = (0, 0, B_0)
\end{equation}$$

Insert this into the equation of motion we have derived

$$\begin{equation}
\frac{dM}{dt} = \gamma\, M \times B_0
\end{equation}$$

Write \( M = (M_x, M_y, M_z) \). Then the cross product gives:

$$\begin{equation}
M \times B_0 
= 
\begin{pmatrix}
M_x \\ M_y \\ M_z
\end{pmatrix}
\times
\begin{pmatrix}
0 \\ 0 \\ B_0
\end{pmatrix}
=
\begin{pmatrix}
M_y B_0 \\
- M_x B_0 \\
0
\end{pmatrix}
\end{equation}$$

So

$$\begin{equation}
\frac{dM_x}{dt} = \gamma\, M_y B_0
\end{equation}$$

$$\begin{equation}
\frac{dM_y}{dt} = -\gamma\, M_x B_0
\end{equation}$$

$$\begin{equation}
\frac{dM_z}{dt} = 0
\end{equation}$$

Key points:

- \(M_z\) is **constant** in time.
- \(M_x\) and \(M_y\) rotate in the \(xy\)-plane.

We can combine these into a single complex variable

$$\begin{equation}
M_\perp = M_x + i M_y
\end{equation}$$

Then

$$\begin{equation}
\frac{dM_\perp}{dt} 
= \frac{d}{dt}(M_x + iM_y)
= \gamma B_0 M_y + i(-\gamma B_0 M_x)
= -i \gamma B_0 (M_x + iM_y)
= -i \gamma B_0 M_\perp
\end{equation}$$

This differential equation has solution

$$\begin{equation}
M_\perp(t) = M_\perp(0)\, e^{-i \omega_0 t}
\end{equation}$$

where

$$\begin{equation}
\boxed{
\omega_0 = \gamma B_0
}
\end{equation}$$

is the **Larmor frequency**.

Geometrically, this means that \( M \) **precesses** around \( B_0 \) at angular frequency \( \omega_0 \).

> **Placeholder for figure:**  
> *[Fig. 2: Magnetization vector precessing around the \(z\)-axis at \(\omega_0\).]*

---

### 1.4 Adding an RF Field: Pulsed NMR and the Rotating Frame

In a pulsed NMR experiment, we apply an additional **radiofrequency (rf) field** using a coil. This field is typically:

- oscillating at frequency \( \omega_{\text{rf}} \),
- perpendicular to \(B_0\),
- of magnitude \(B_1\) (much smaller than \(B_0\)).

A convenient idealization:

$$\begin{equation}
B_1(t) = (B_1 \cos \omega_{\text{rf}} t,\; B_1 \sin \omega_{\text{rf}} t,\; 0)
\end{equation}$$

The **total field** is then:

$$\begin{equation}
B(t) = B_0 + B_1(t)
\end{equation}$$

Solving the exact motion in the lab frame is possible but messy. Instead, NMR uses the **rotating frame**.

---

#### 1.4.1 The Rotating Frame Idea

Imagine looking at the system from a coordinate frame rotating around the \(z\)-axis at angular frequency \( \omega_{\text{rf}} \). In this frame:

- The oscillating \( B_1(t) \) can become approximately **static** if we choose the rotation properly.
- The large static field \(B_0\) is partly transformed away.

Mathematically, in the rotating frame the magnetization obeys:

$$\begin{equation}
\frac{dM}{dt}\Big|_{\text{rot}} = 
\gamma\, M \times B_{\text{eff}}
\end{equation}$$

where the **effective field** is

$$\begin{equation}
B_{\text{eff}} =
\left(
B_1,\; 0,\; \frac{\omega_0 - \omega_{\text{rf}}}{\gamma}
\right)
\end{equation}$$

(Here we’ve chosen \(B_1\) along the rotating-frame \(x\)-axis for simplicity.)

Special case: **on-resonance pulse**

- If \( \omega_{\text{rf}} = \omega_0 \) then
  $\frac{\omega_0 - \omega_{\text{rf}}}{\gamma} = 0$ so  $B_{\text{eff}} = (B_1, 0, 0)$
- The magnetization precesses **purely around the \(x\)-axis** at angular frequency \( \omega_1 = \gamma B_1 \).

---

#### 1.4.2 Flip Angle and Pulse Duration

In the rotating frame, if \( M \) starts along the \(+z\)-axis and the rf field \( B_1 \) is applied along \(+x\), \( M \) simply **rotates around the \(x\)-axis**.

The **nutation frequency** (rate of rotation about \(B_1\)) is:

$$\begin{equation}
\omega_1 = \gamma B_1
\end{equation}$$

If the pulse is applied for a duration \(t_{\text{rf}}\), the **flip angle** (rotation angle) is:

$$\begin{equation}
\boxed{
\theta_{\text{rf}} = \omega_1 t_{\text{rf}} = \gamma B_1 t_{\text{rf}}
}
\end{equation}$$

Important special cases:

- **\(90^\circ\) pulse** (or \(\pi/2\) pulse):

$$\begin{equation}
  \theta_{\text{rf}} = \frac{\pi}{2} \quad \Rightarrow \quad t_{\pi/2} = \frac{\pi}{2 \gamma B_1}
\end{equation}$$

  Rotates \(M\) from \(+z\) into the transverse plane (e.g. along \(-y\) if pulse is along \(+x\)).<br/><br/>

- **\(180^\circ\) pulse** (or \(\pi\) pulse):

$$\begin{equation}
  \theta_{\text{rf}} = \pi \quad \Rightarrow \quad t_{\pi} = \frac{\pi}{\gamma B_1}
\end{equation}$$

  Inverts \(M\) from \(+z\) to \(-z\).

> **Placeholder for figure:**  
> *[Fig. 3: Effect of a \(90^\circ_x\) pulse: magnetization initially along \(z\) tipped into \(-y\) direction in rotating frame.]*

After the pulse, if we **stop** the rf field, then \( B_{\text{eff}} \) reduces back to just the effective static field, and in the rotating frame the magnetization either:

- remains fixed (if on resonance), or
- precesses about \(z\) at \( \omega_0 - \omega_{\text{rf}} \) (if off resonance).

---

## 2. Quantum Model of NMR

The classical description works well for **intuition** and for systems of many **uncoupled** spins.  
But to describe **interactions** (dipolar couplings, quadrupolar effects, etc.) and to derive everything from first principles, we need the **quantum mechanical model**.

---

### 2.1 Single Spin, Wavefunctions, and Operators

Quantum mechanically, a **single spin** is described by a **state vector** (wavefunction) \( |\psi\rangle \), and physical quantities are represented by **operators**.

For one spin:

- The spin angular momentum operator is \( \hat{I} = (\hat{I}_x, \hat{I}_y, \hat{I}_z) \).
- The **magnitude** squared is \( \hat{I}^2 \).

For a spin with quantum number \( I \):

- \( \hat{I}^2 |I, m\rangle = \hbar^2 I(I+1) |I, m\rangle \)
- \( \hat{I}_z |I, m\rangle = \hbar m |I, m\rangle \)

with

- \( I = 0, \frac{1}{2}, 1, \frac{3}{2}, \dots \)
- \( m = I, I-1, \dots, -I \)

For NMR, the most common case is **spin-½**:

- \( I = \frac{1}{2} \)
- \( m = +\frac{1}{2} \) (often called \( |\alpha\rangle \)) or \( m = -\frac{1}{2} \) (often called \( |\beta\rangle \)).

---

### 2.2 Zeeman Hamiltonian: Spin in a Static Magnetic Field

A magnetic moment operator \( \hat{\mu} \) is proportional to the spin operator:

$$\begin{equation}
\hat{\mu} = \gamma \hbar \hat{I}
\end{equation}$$

Place the spin in a **static magnetic field** \( B_0 = (0, 0, B_0) \).  
The energy (Hamiltonian operator) is

$$\begin{equation}
\hat{H} = - \hat{\mu} \cdot B_0
       = - \gamma \hbar \hat{I} \cdot (0,0,B_0)
       = - \gamma \hbar B_0 \hat{I}_z
\end{equation}$$

We now find the **energy eigenstates** by applying \( \hat{H} \) to \( |I, m\rangle \):

$$\begin{equation}
\hat{H} |I, m\rangle = - \gamma \hbar B_0 \hat{I}_z |I, m\rangle
                     = - \gamma \hbar B_0 (\hbar m) |I, m\rangle
                     = E_{I,m} |I, m\rangle
\end{equation}$$

Thus

$$\begin{equation}
\boxed{
E_{I,m} = - \gamma \hbar^2 B_0\, m
}
\end{equation}$$

Often the factor of \( \hbar \) is absorbed into the definition of \( \hat{I}_z \), so one writes simply

$$\begin{equation}
E_{I,m} = - \gamma \hbar B_0 m
\end{equation}$$

For a **spin-½**:

- \( m = +\frac{1}{2} \): \( E_{+} = - \frac{1}{2} \gamma \hbar B_0 \)
- \( m = -\frac{1}{2} \): \( E_{-} = + \frac{1}{2} \gamma \hbar B_0 \)

The **energy splitting** between the two levels is

$$\begin{equation}
\Delta E = E_{-} - E_{+} = \gamma \hbar B_0
\end{equation}$$

Dividing by \( \hbar \), we get a frequency:

$$\begin{equation}
\frac{\Delta E}{\hbar} = \gamma B_0 = \omega_0
\end{equation}$$

This is exactly the **Larmor frequency** we found in the classical picture.

> **Important connection:**  
> Classical precession frequency of \( M \) = quantum transition frequency between Zeeman levels.

---

### 2.3 Thermal Populations and Ensemble Magnetization

Real samples contain **many spins**. At nonzero temperature, spins occupy the energy levels with **Boltzmann probabilities**.

For a level with energy \( E_y \), the equilibrium population fraction is

$$\begin{equation}
p_y = \frac{e^{-E_y / k_B T}}{\sum_{y'} e^{-E_{y'} / k_B T}}
\end{equation}$$

For a spin-½ in a field \( B_0 \):

- Upper level \( |\beta\rangle \) (higher energy): \( E_{-} \)
- Lower level \( |\alpha\rangle \) (lower energy): \( E_{+} \)

Since \( \Delta E = \gamma \hbar B_0 \) is typically **much smaller** than \( k_B T \), we can approximate populations, but for now we keep the exact expression.

The **z-component** of the magnetic moment operator is

$$\begin{equation}
\hat{\mu}_z = \gamma \hbar \hat{I}_z
\end{equation}$$

The **ensemble-averaged magnetization** along \(z\) is

$$\begin{equation}
\langle M_z \rangle = N \langle \hat{\mu}_z \rangle
\end{equation}$$

where \(N\) = number of spins, and

$$\begin{equation}
\langle \hat{\mu}_z \rangle 
= \sum_y p_y \langle y | \hat{\mu}_z | y \rangle
\end{equation}$$

For spin-½, there are only two states: \( |\alpha\rangle \) and \( |\beta\rangle \).

- \( \hat{I}_z |\alpha\rangle = +\frac{\hbar}{2} |\alpha\rangle \)
- \( \hat{I}_z |\beta\rangle = -\frac{\hbar}{2} |\beta\rangle \)

Therefore

$$\begin{equation}
\langle \hat{\mu}_z \rangle 
= p_\alpha \gamma \hbar \left(+\frac{1}{2}\right)
+ p_\beta \gamma \hbar \left(-\frac{1}{2}\right)
= \gamma \hbar \frac{1}{2}(p_\alpha - p_\beta)
\end{equation}$$

so the macroscopic magnetization is

$$\begin{equation}
\boxed{
\langle M_z \rangle = N \gamma \hbar \frac{1}{2} (p_\alpha - p_\beta)
}
\end{equation}$$

Thus:

- The **nonzero net magnetization** comes from the **slight excess** of spins in the lower energy state.
- This quantum population difference is what the **classical vector** \( M \) really represents.

---

### 2.4 Classical vs Quantum: Summary

- **Classical picture**
  - Describes the **time evolution** of a macroscopic magnetization vector \( M \).
  - Magnetization precesses around \( B_0 \) at \( \omega_0 = \gamma B_0 \).
  - RF pulses **rotate** \( M \) by an angle \( \theta_{\text{rf}} = \gamma B_1 t_{\text{rf}} \) in the rotating frame.

- **Quantum picture**
  - Describes each **nuclear spin** with states \( |I,m\rangle \) and a Hamiltonian \( \hat{H} = - \gamma \hbar B_0 \hat{I}_z \).
  - Energy levels are split by \( \Delta E = \gamma \hbar B_0 \).
  - Net magnetization arises from slight **Boltzmann imbalance** between populations.

They are two ways of telling the **same story**:

- Classical: a **precessing arrow** in space.
- Quantum: **two (or more) energy levels** with populations and transitions.

Both are essential to solid-state NMR:
- We often **think** in the classical picture,
- But we **calculate** complicated experiments in the quantum (density operator) language.


*End of Lecture 1.*

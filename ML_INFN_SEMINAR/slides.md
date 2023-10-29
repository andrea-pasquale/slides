---
theme: seriph
class: text-center
coverDate: ""
lineNumbers: false
drawings:
  persist: false
transition: slide-left
# css: unocss
title: Towards a hybrid quantum operating system
css: unocss
hideInToc: truehide
themeConfig:
  primary: '#4d7534'
layout: intro
---

# Towards a hybrid quantum operating system

Andrea Pasquale on the behalf of the Qibo collaboration

30th October 2023, ML_INFN

<table>
        <tr>
            <td><img src="tii.png" alt="Figure 1" width="100"></td>
            <td><img src="Logo_Università_degli_Studi_di_Milano.png" alt="Figure 2" width="100"></td>
            <td><img src="2560px-INFN_logo_2017.png" alt="Figure 3" width="100"></td>
        </tr>
    </table>

---
hideInToc: truehide
layout: default
---

# Table of contents

<Toc maxDepth="1"></Toc>

---
transition: slide-up
level: 1
layout: section
---

# What is Quantum Computing?

---
hideInToc: truehide
layout: quote
---

## “Nature isn’t classical dammit, and if you want to make a simulation of Nature you better make it quantum mechanical, and by golly it’s a wonderful problem because it doesn’t look so easy.”

Richard Feynman



---
layout: two-cols
---

## Quantum Superposition

Compared to classical bits which can
be represented by a single state (0 or 1), quantum
bits can be prepared in any superposition of $\ket{0}$ or $\ket{1}$.

$$
\ket{\psi} = \alpha \ket{0} + \beta \ket{1}
$$
where $\alpha, \beta \in \mathcal{C} : |\alpha|^2 + |\beta|^2= 1$.

In the case of a system with two qubit we obtain the following representation:

$$
\ket{\psi} = \alpha_{00} \ket{00} + \alpha_{01} \ket{01} + \alpha_{10} \ket{10} + \alpha_{11} \ket{11}
$$
with a similar normalization condition:

$$\sum_{i,j=0,1} |\alpha_{ij}|^2 = 1$$
<template v-slot:right>
<br>
<br>
  <figure class="right-align">
   <img src="/Qubit-1.png" width="400" >
   </figure>
</template>

---

## Quantum Entanglement

In QC we can extract correlations between different qubits through *entanglement*.

For example if we consider the following state[^1]:

$$
\ket{\Phi^+} = \frac{\ket{00} + \ket{11}}{\sqrt{2}}
$$
whenever we measure $\ket{0}$ for the first qubit also the second one
will be measured in $\ket{0}$ and the same goes for $\ket{1}$.

<br>
<img src="/entanglment.png" width="400" >


[^1]: This state is known as Bell state, an example of maximally entangled state.

---
layout: two-cols
---

## How to modify the state of a Qubit?

Being a quantum system qubits $\ket{\psi}$ evolve over time through unitary operators $U_i$.

$$
\ket{\psi'} = U_1 U_2 \ket{\psi}
$$

Maintaining an analogue to classical quantum we can represent the following evolution as a small quantum circuit:

<img src="/single_qubit_circuit.png" width="300" >

which we call **quantum circuit**.

Contrary to classical circuit only **reversible** gates can be used in quantum circuits




::right::
<br>
<br>

<img src="/gates_wiki.png" width="300" >




---

## Quantum circuits

A quantum circuits with more than one qubit can be represented as:

<img src="/multi_qubit_circuit.png" width="450" >

**Exponential scaling**

All the possible initial states for a system of 3 qubits are $2^3$, in fact a generic unitary
for this system is $8 \times 8$ matrix.

Increasing the number of qubits leads to exponential scaling of the system $\Rightarrow$ **more expressivity!**
---
layout: section
---

# A snapshot of Quantum Machine Learning

---

## Variational Quantum Circuits

How can we encode information in a quantum circuits?

**Variational Quantum Circuit**: quantum circuits with parametrized gates.

<img src="vqc.png" height="600" width="600">

Observation: We can use the Variational Quantum Circuits as as a **neural network**.

---
layout: two-cols
---

## Rational

Using Variational Quantum Circuits we can define a Variational Quantum Computer!

1. we want a quantum circuit $\mathcal{U}(\theta)$ to **approximates** some law $V$.
1. executing $\mathcal{U}(\theta)$ we use a **variational quantum state** to reach the solution
1. **Solovay-Kitaev theorem**: the number of gates needed by $\mathcal{U}$ to represent $V$
with precision $\delta$ is $\mathcal{O}(\log^c \delta^{-1})$, where $c < 4$.


<template v-slot:right>
<br>
<br>
<img src="variational.png" alt="Figure 1" width="400">
</template>

---

## ML workflow for QML

Now we can treat a VQC as a neural network in order to optimize the parameters through back-propagation.

<img src="ml_scheme.png" height="600" width="600">

---

## Why QML?

<ul>
<li><p><b>shallow models</b> thanks to superposition and entanglement</p></li>
<li><p>map problems into Hilbert’s spaces loads to high <b>expressivity</b></p></li>
<li><p>exploit QC sub-routines to <b>speed-up</b> classical algorithms (e.g. using
Grover)</p></li>
<li><p>physical advantages when dealing with <b>combinatorial optimization</b>
(quantum annealing)</p></li>
</ul>
<br>
<br>
<table>
        <tr>
            <td><img src="block_sphere.png" alt="Figure 1" width="500"></td>
            <td><img src="shallow.png" alt="Figure 2" width="500"></td>
            <td><img src="speedup.png" alt="Figure 3" width="500"></td>
            <td><img src="tunneling.jpg" alt="Figure 4" width="500"></td>
        </tr>
    </table>






---
layout: section
---

# Introducing Qibo
Open-source full stack API for quantum simulation, hardware control and calibration

---

## Why do you need a framework?

<img src="map.svg" height="600" width="600">

<p align="center">
<em> Is it possible to create from scratch a framework for all of this?</em>
</p>


---

## Qibo: a brief overview





<img src="/qibo_ecosystem.svg" class="center" height="700" width="700">

<p align="right">
<a href="https://qibo.science/">Qibo documentation</a>
</p>

---
layout: two-cols
---
## Gate set abstraction


```python
import numpy as np
from qibo.models import Circuit
from qibo import gates, set_backend

# Set driver engine
set_backend("numpy")

c = Circuit(2)
c.add(gates.X(0))

# Add a measurement register on both qubits
c.add(gates.M(0, 1))

# Execute the circuit with the default initial state |00>.
result = c(nshots=100)
```

```python
# Change backend
set_backend("qibojit")

# Circuit execution with new driver
result = c(nshots=100)
```



<template v-slot:right>

## Qibo features

<ul>
<li><p>Definition of a <b> standard language </b> for the construction and execution of quantum circuits with <b>device agnostic approach</b> to simulation and quantum hardware control based on plug and play backend drivers.</p></li>
<li><p>A <b>continuously growing</b> code-base of quantum algorithms and applications presented with examples and tutorials.</p></li>
<li><p><b>Efficient simulation</b> backends with GPU, multi-GPU and CPU with multi-threading support.</p></li>
<li><p>A simple mechanism for adding <b>new simulation and hardware backend drivers</b>.</p></li>
</ul>

<p align="right">
<a href="https://arxiv.org/abs/2009.01845">2009.01845</a>
</p>
</template>

---

## High performance simulation

❌ <b>Long</b> computational times using naive approach (`Numpy` or `TensorFlow`) for circuits with <b>large</b> number of qubits.

✅ We need more sophisticated tools to be able to simulate a quantum circuits with more qubits!

<p align="center">
<img src="/Qibojit.png" alt="Qibojit" height="700" width="700">
</p>



<p align="right">
<a href="https://arxiv.org/abs/2203.08826">2203.08826</a>
</p>


---

## Benchmark


<p align="center">
<img src="30_qubits.png-1.png" alt="Qibo">
</p>

All the benchmarks are available in [qibojit-benchmarks](https://github.com/qiboteam/qibojit-benchmarks).

---
layout: section
---


# Quantum Machine Learning examples using Qibo


---
level: 1
transition: slide-up
layout: center
class: text-center
---

# Monte Carlo event generator using Quantum GAN

---
layout: two-cols

---
## What are Generative Adversarial Networks?



<p align="center">
<img src="gan_image.jpg" alt="Qibo" height="500" width="500">
</p>


<template v-slot:right>

**Training**

Adapt alternatively the generator $G(\phi_g , z)$ and the discriminator $D(\phi_d , x)$

**Metrics**

Binary cross-entropy for the loss functions:

$\mathcal{L}_G(\phi_g,\phi_d) = -\mathbb{E}_{z \sim p_{\mathrm{prior}}(z)}[\log D(\phi_d,G(\phi_g,z))]$

$\mathcal{L}_D(\phi_g,\phi_d) = \mathbb{E}_{x \sim p_{\mathrm{real}}(x)}[\log D(\phi_d,x)] +\, \mathbb{E}_{z \sim p_{\mathrm{prior}}(z)}[\log (1-D(\phi_d,G(\phi_g,z)))]$

**Game theory**: min-max two-player game to reach Nash equilibrium

$\underset{\phi_g}{\min}\,\,\mathcal{L}_G(\phi_g,\phi_d)  \quad  \underset{\phi_d}{\max}\,\,\mathcal{L}_D(\phi_g,\phi_d)$


</template>

---
layout: two-cols
---

## A classical-quantum approach


We replace the classical generator using a VQC:

<p align="center">
<img src="ansatz1-1.png" alt="Qibo" height="400" width="400">
</p>
 with the following encoding of the noise:

 $$R^{l,m}_{y,z}\left(\vec{\phi_g}, \vec{z}\right) = R_{y,z}\left(\phi_g^{(l)} z^{(m)} + \phi_g^{(l-1)}\right)$$

 we sample according to

$$x_i = \bra{\Psi_{\phi_g}(\vec{z})} \sigma_z^i \ket{\Psi_{\phi_g}(\vec{z})}$$

<p align="right">
<a href="https://arxiv.org/abs/2110.06933">2110.06933</a>
</p>

<template v-slot:right>
<p align="center">
<img src="scheme2-1.png" alt="Qibo" height="400" width="400">
</p>

</template>

---

## Validation: 3D correlated Gaussian function

<img src="gaussian-1.png" alt="Qibo" height="500" width="500">

---

## A more challenging example: $pp \rightarrow t \bar{t}$

<p align="right">
Simulation
</p>

<p align="center">
<img src="sty.png" alt="Qibo" height="500" width="500">
</p>


---

## Results with $pp \rightarrow t \bar{t}$ execution on hardware

<p align="right">
Execution on Hardware
</p>

<img src="real-sty.png" alt="Qibo" height="500" width="500">

---

# Determining the proton content with a quantum computer

##

In this work a parametrized Variational Quantum Circuit (VQC) is employed
to fit Parton Density Functions (PDFs)[^1]:


$$ \text{qPDF}_i (x, Q_0, \theta)$$

where $x$ is the momentum fraction of the hadron carried by the parton $i$ at fixed
energy scale $Q_0$ while $\theta$ are parameters of the VQC.

<p align="center">
<img src="pdf.png" class="center" height="400" width="400">
</p>

<p align="right">
<a href="https://arxiv.org/abs/2011.13934">2011.13934</a>
</p>

This is just one of the possible application of QC/QML in High Energy Physics!
[^1]:

---
layout: two-cols
---

## Methodology

The model is created as follows:

1. Inject $x$ in VQC: $\mathcal{U}(\theta) \rightarrow \mathcal{U}(\theta,x)$[^1]
1. Extract information from QC through series of Hamiltonians:
$$ Z_i = \bigotimes_{j=0}^{n} Z^{\delta_{ij}} $$
3. Define the function:
$$ z_i(\theta, x) = \bra{\psi} \mathcal{U}^\dagger (\theta, x) Z_i \mathcal{U}(\theta,x) \ket{\psi}$$
4. Finally we define the qPDF model for flavour $i$ at a given $(x, Q_0)$ as
$$ \text{qPDF}_i(x, Q_0, \theta) = \frac{1-z_i(\theta, x)}{1 +z_i(\theta, x)}$$


[^1]: A possible choice for this is the re-uploading procedure

<template v-slot:right>

<figure>
<br>
<br>
<img src="new_circuits.png" alt="Qibo">
<figcaption>Representation of a single layer used.</figcaption>
</figure>

</template>

---

<figure>
<img src="Experiments-SingleFlavor-1.png" alt="Qibo">
<figcaption>Top: Single-flavour fit for all flavours for 5 layers and 8 qubits.
The red lines are the prediction of the qPDF model with simulated noise from IBM processor.
Green points are results from running on actual quantum hardware from IBM.
Bottom: Fit results for the gluon and the u and s quarks. qPDF is able to reproduce the features
of NNPDF3.1. We now see this is also true when the fit performed by comparing to data and not by comparing directly to the
goal function. The differences seen at low-x can be attributed to the lack of data in that region. </figcaption>

</figure>

<table>
        <tr>
            <td><img src="gluon-1.png" alt="Figure 1" width="500"></td>
            <td><img src="uquarks-1.png" alt="Figure 2" width="500"></td>
            <td><img src="squarks-1.png" alt="Figure 3" width="500"></td>
        </tr>
    </table>


---
level: 1
---

# Probability density function determination using adiabatic quantum annealing

<p align="right">
<a href="https://arxiv.org/abs/2303.11346">2303.11346</a>
</p>

The goal of this work is to estimate  the probabilitiy density function value $\rho(x)$ for each element of a sample of data $\omega = \{x_i \}_{i=1}^{N_{data}}$ using the following
Quantum Adiabatic Machine Learning (QAML) strategy:


* encoding the CDF values $F(x)$ inoto an adiabatic evolution

* translating the adiabatic Hamiltonian into a circuit $\mathcal{C}$ callable at any time $\tau$

* Derivating the circuit using the parameter shift rule[^1] obtaining the PDF



[^1]: The Parameter Shift Rule (PSR)[https://arxiv.org/abs/1905.13311] is an algorithm to compute the derivative of specific quantum circuits which is hardware compatible

---
layout: two-cols
---

## Model regression with QAML


Given a one-dimensional function $f(t)$ we can choose two Hamiltonians $H_0$ and $H_1$ such that we have
$f(t_0) = \braket{H_0}$ and $f(t_1) = \braket{H_1}$. Therefore we need to find a time-dependent Hamiltonian
$H(t)$ such that

$$ \braket{H(t)} = f(t) $$

We can create a parametric model for this Hamiltonian by using quantum annealing with a parametric scheduler:

$$ H(t) = \Big[ 1 - s(t,\theta)\Big] H_0 + s(t, \theta) H_1$$

We can then use standard ML tools to train $H(t)$ by optimizing the parameters $\theta$ in the scheduling $s(t, \theta)$.


<template v-slot:right>

<figure>
<br>
<br>

<img src="evolution.png" alt="Qibo">
<figcaption> Example of initial and final state of the algorithm. The
Ntrain blue points are the training set selected from a gaussian
mixture sample, whose empirical CDF is represented by the black
line. The random initialization of the adiabatic evolution leads
to the initial sequence of energies (yellow line). After a training
time, the evolution is closer to the training set (red line).</figcaption>
</figure>

</template>

---
layout: two-cols
---

## Translation of Hamiltonian into derivable Circuit

**Sketch of the algorithm**

1. Diagonalization of each $H_j$

$$ \mathcal{C_n} = \prod_{j=0}^{n}e^{id\tau H_j} =  \prod_{j=0}^{n} P_j e^{id\tau D_j} P_j^{-1}$$

2. Take the limit $d\tau\rightarrow 0$

$$C(t) = P_t \exp \Big( i \int_0^{t/T} D_j d\tau \Big) P_0^{-1}$$

3. Rewrite circuit as compositon of rotations and apply PSR

$$\text{PDF}(t) = \text{PSR}{\braket{\mathcal{C}(t)^\dagger Z \mathcal{C(t)}}}$$
<br>

<template v-slot:right>
<br>
<br>
<br>

<img src="AdiabaticPSR.png" alt="Qibo" class="center" height="600" width="600">




</template>

---

## Validation

<figure>
<img src="PDF_gauss_30_20_200000-1.png" alt="Qibo" class="center" height="700" width="700">
</figure>


---
layout: section
---
# Introducing Qibolab
Quantum control for self-hosted QPUs using Qibo

---

## Motivation I: Gate to Pulse conversion

Usually a quatum computer will be able to produce only a few gates called **native gates**
through a specific series of pulses generated by dedicated instruments.
A physical implementation of a quantum gate requires the conversion of a matrix to
a sequence of microwave signals:

<img src="map1.png" height="100">

It can be shown that any arbitrary single qubit can be written in terms on native gates $R_X$ and $R_Z$:

$$U_3(\theta, \phi, \lambda)=\begin{pmatrix}
\cos{\frac{\theta}{2}} & - e^{i \lambda} \sin{\frac{\theta}{2}} \\
e^{i \phi} \sin{\frac{\theta}{2}} & e^{i(\phi+\lambda)} \cos{\frac{\theta}{2}}
\end{pmatrix} \rightarrow R_Z(\phi) R_X(-\pi/2) R_Z(\phi) R_X(\pi/2) R_Z(\phi)$$

This proof can be generalized for multi-qubit gates.
---

## Motivation II: Circuit transpilation

Usually a quantum computer will have a specific connectivity:



<img src="transpilation.png" height="80" width="300">

To be able to execute arbitrary circuits we need to rewrite the circuit in a way
that matches the topology of the specific quantum devices. Therefore we need also all
the tools to be able to perform these graph manipulations which usually we refer to as
**transpiler**.


<img src="example_transpilation.png" height="100" width="700">


---
layout: two-cols
---

## Qibolab API

<br>
<br>
Some of the key features of Qibolab are:

<ul>
<li><p>Platform API: support custom allocation of quantum hardware platforms/ lab setup.</p></li>
<li><p>Drivers: support commercial and open-source firmware for hardware control</p></li>
<li><p>Pulse API: provide a library of custom pulses for execution through instruments</p></li>
<li><p>Quantum circuit deployment: seamlessly deploys quantum circuit models on quantum hardware</p></li>

</ul>




<template v-slot:right>
<br>
<img src="instruments.svg" alt="Qibo" height="500" width="500">

<p align="right">
<a href="https://arxiv.org/abs/2308.06313">2308.06313</a>

<a href="https://arxiv.org/pdf/2310.05851">2310.05851</a>

</p>
</template>

---
layout: two-cols
---
## Pulse API example

```python
from qibolab import create_platform
from qibolab.pulses import ReadoutPulse, PulseSequence

# Define PulseSequence
sequence = PulseSequence()
# Add some pulses to the pulse sequence
sequence.add(ReadoutPulse(start=0,
                   amplitude=0.3,
                   duration=4000,
                   frequency=200_000_000,
                   shape='Gaussian(5)'))

# Define platform
platform = create_platform("tii1q_b1")
# Platform setup
platform.connect()
platform.setup()
platform.start()
# Executes a pulse sequence.
results = platform.execute_pulse_sequence()
platform.stop()
platform.disconnect()
```
<template v-slot:right>

## Drivers implemented

Currently Qibolab supports the following drivers:

<ul>
<li><p>Qblox</p></li>
<li><p>Quantum Machines</p></li>
<li><p>Zurich Instruments</p></li>
<li><p> RFSoC (based on Qick)</p></li>

</ul>

We also support local oscillators
<ul>
<li><p>RohdeSchwarz SGS100A</p></li>
<li><p>ERASynth</p></li>
</ul>

<p align="right">
<a href="https://github.com/qiboteam/qibolab">Qibolab</a>
</p>
</template>

---
transition: slide-up
layout: section
---
# Introducing Qibocal
Quantum calibration and characterization using Qibo

---

## Motivation

Let's suppose the following:
<ol>
<li><p>We have a QPU (self-hosted).</p></li>
<li><p>We have control over what we send to the QPU.</p></li>
<li><p>We know how to convert quantum circuits to pulses. </p></li>
</ol>

Can I **trust** my results? **NO!**


**Characterization** and **calibration** are an essential step to properly operate emerging quantum devices.


<p align="center">
<img src="/rabi1.png" alt="Characterization" width="1000" height="100" >
<em> Calibration of RX pulse amplitude through a Rabi experiment through Qibocal</em>.
</p>


---
layout: two-cols
---

## Qibocal API

<br>
<br>
<br>
Qibocal key features:

<ul>
<li><p>Automation of calibration protocol</p></li>
<li><p>Declarative inputs using runcards</p></li>
<li><p>Generation of HTML reports</p></li>
<li><p>API to run protocols directly in python</p></li>
</ul>

<template v-slot:right>
<br>
<br>

<p align="center">
<img src="qq_qibocal.svg" alt="Characterization" height="600" width="600">
</p>
<p align="right">
<a href="https://arxiv.org/abs/2303.10397">2303.10397</a>
</p>
</template>

---

## Qubit characterization

<p align="center">
<img src="qpu_characterization.svg" alt="Characterization" height="800" width="800">
</p>

---
layout: section
---

# Outlook

---
layout: two-cols
---

We have presented **Qibo**, a simple full stack API capable of

<ul>
<li><p>Deploying <a href="https://qibo.science/qibo/stable/code-examples/applications.html#quantum-machine-learning">QML algorithms:</a></p></li>
<li><p>Simulating large circuits in an efficient way: <a href="https://github.com/qiboteam/qibojit">qibojit</a></p></li>
<li><p>Deploying circuits in self-hosted QPUs: <a href="https://github.com/qiboteam/qibolab">qibolab</a></p></li>
<li><p>Calibrating self-hosted QPUs: <a href="https://github.com/qiboteam/qibocal">qibocal</a></p></li>
</ul>


Why should you choose Qibo?

<ul>
<li><p>Publicly available as an <b>open source</b> project</p></li>
<li><p>Community driven effort</p></li>
<li><p>Specifically designed for <b>self-hosted QPUs</b> </p></li>

</ul>

<template v-slot:right>
<p align="center">
<img src="github.png" alt="Characterization" height="800" width="800">
</p>
</template>

---

# Collaborators

<img src="qibo_world.png" alt="Characterization" height="800" width="800">

---
layout: section
---

# Thanks for listening!
## Questions time.
---
theme: default
class: text-center
lineNumbers: false
drawings:
  persist: false
transition: slide-left
# css: unocss
title: Towards a hybrid quantum operating system
css: unocss
---

# Towards a hybrid quantum operating system

Andrea Pasquale on the behalf of the Qibo collaboration

9th May 2023, CHEP2023, Norfolk

<br>
<br>
<div class="row">
  <div class="column3">
    <img src="figures/TII.jpg" alt="Snow" style="width:50%">
  </div>
  <div class="column3">
    <img src="figures/Logo_Università_degli_Studi_di_Milano.png" alt="Forest" style="width:30%">
  </div>
  <div class="column3">
    <img src="figures/2560px-INFN_logo_2017.png" alt="Mountains" style="width:50%">
  </div>
</div>
---
transition: fade-out
layout: two-cols
---

## Why Quantum Computing in HEP?

LHC produces TB of data per second which need to processed.

Currently we **cannot** keep up with the computational resources required!


Quantum Computing (QC), especially Quantum Machine Learning (QML), is a promising solution:

<ul>
<li><p>Efficient in <b> high-dimensional </b> quantum state spaces</p></li>
<li><p>Potential computational speed-ups</p></li>
<li><p>Qubit representation offers <b>better compression </b></p></li>
<li><p>Acceleration of <b> classically expensive </b> operations (eigensolvers).</p></li>
</ul>

<template v-slot:right>
<p align="right">
<a href="https://www.nature.com/articles/s42254-022-00425-7">Nat Rev Phys 4, 143–144</a>
</p>
<p align="center">
<img src="figures/ATLAS.png" alt="Intro" width="600" height="300">
<figcaption>Projected compute usage from ATLAS Software and Computing HC-LHC Roadmap</figcaption>
</p>
</template>

---

# How can we start using Quantum Computing?

## 
<p align="center">
<img src="figures/Challenge scheme.png" alt="Intro" width="450" height="400">
<em> Is it possible to create from scratch a framework for all of this?</em>
</p>




---
transition: slide-up

layout: center
class: text-center

---

# Introducing Qibo

Open-source full stack API for quantum simulation, hardware control and calibration

---
transition: slide-left
---

<p align="center">
<img src="figures/fig1_qibocal.png" alt="Qibo">
</p>

---
transition: slide-left


layout: center
class: text-center
---

# Simulation

---
layout: two-cols
---
# Gate set abstraction
## 

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

# High performance simulation
##

❌ <b>Long</b> computational times using naive approach (`Numpy` or `TensorFlow`) for circuits with <b>large</b> number of qubits.

✅ We need more sophisticated tools to be able to simulate a quantum circuits with more qubits! 

<p align="center">
<img src="figures/Qibojit.png" alt="Qibojit" height="700" width="700">
</p>



<p align="right">
<a href="https://arxiv.org/abs/2203.08826">2203.08826</a>
</p>


---

# Benchmark
##

<p align="center">
<img src="figures/30_qubits.png-1.png" alt="Qibo">
</p>

All the benchmarks are available in [qibojit-benchmarks](https://github.com/qiboteam/qibojit-benchmarks).

---
layout: center
class: text-center
---

# Qibolab
Automatic deployment of quantum circuits on quantum hardware


---
layout: two-cols
---

<template v-slot:default>

## Motivation
```mermaid
flowchart TD
    A[How are gates implemented on a quantum computer?] 
```
```mermaid
flowchart TD
    B[By sending microwave pulses.]
```
```mermaid 
flowchart TD
    C[How do we control them?]
```
```mermaid
flowchart TD
    D[Using FPGAs]
```
&rarr; We need both a pulse level API + drivers to interface Qibo with different instruments.

</template>
<template v-slot:right>
<p align="center">
<img src="figures/qblox.png" alt="Qibo"  width="200" height="200">

<img src="figures/rohde.jpg" alt="Qibo"
width="200" height="200" >

<img src="figures/fpga.png" alt="Qibo" width="200" height="200">
</p>
</template>


---
layout: two-cols
---
## Pulse API example
```python
from qibolab import Platform
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
platform = Platform("tii1q_b1")
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
<li><p> FPGAs (based on Qick)</p></li>

</ul>

We also support local oscillators
<ul>
<li><p>RohdeSchwarz SGS100A</p></li>
<li><p>ERASynth</p></li>
</ul>
</template>

---
transition: slide-up
layout: center
class: text-center
---


# Introducing Qibocal
A reporting tool for calibration using Qibo


---

# Motivation
## 
Let's suppose the following:
<ol>
<li><p>We have a QPU (self-hosted).</p></li>
<li><p>We have control over what we send to the QPU.</p></li>
<li><p>We know how to convert quantum circuits to pulses. </p></li>
</ol>

Can I **trust** my results? **NO!**


**Characterization** and **calibration** are an essential step to properly operate emerging quantum devices.


<p align="center">
<img src="figures/rabi1.png" alt="Characterization" width="1000" height="100" >
<em> Calibration of RX pulse amplitude through a Rabi experiment through Qibocal</em>.
</p>


---

# Single Qubit Characterization: Pulse Level
## 
<p align="center">
<img src="figures/single qubit characterization.png" alt="Characterization" >
</p>


---

# Single Qubit Characterization: Circuit Level
## 

<div class="row">
  <div class="column">
    <img src="figures/RB zoo.png" alt="Snow" style="width:100%">
  </div>
  <div class="column">
    <ul>
    Currently in Qibocal the following protocols are implemented:
    <li><p>Standard RB</p></li>
    <li><p>Simulataneous Filtered RB</p></li>
    <li><p>XId RB</p></li>
    </ul>
  </div>
</div>
<p>  </p>
We are currently developing a suite for the development of the <b> latest </b>
quantum benchmarking protocols available in the <b>literature</b>.



---

# Reporting tools
## 
<div class="row">
  <div class="column">
<img src="figures/Qibocal.png" alt="Characterization" width="500" height="500" >
  </div>
  <div class="column">
  <p align="right">
<a href="https://arxiv.org/abs/2303.10397">2303.10397</a>
</p>
<p> </p >
    <ul>
    <li><p>Platform agnostic approach</p></li>
    <li><p>Launch calibration routines easily</p></li>
    <li><p>Live-plotting tools</p></li>
    <li><p>Autocalibration (under development)</p></li>
    </ul>
  </div>
</div>







---

<img src="figures/report1.png" alt="Qibo">


---
layout: center
class: text-center
---

# Applications

---

## Determination of parton distribution functions using QML  <a href="https://arxiv.org/abs/2011.13934">2011.13934</a>



<p align="center">
<img src="figures/Experiments-SingleFlavor-1.png" alt="Characterization" height="800" width="800">
<img src="figures/ansatz.png" alt="Characterization" height="800" width="700">

</p>
---
layout: two-cols
---

## MonteCarlo event generator using QGAN

<p align="center">
<img src="figures/ansatz1-1.png" alt="Characterization" height="800" width="800">
<br>
<br>
<br>
<em> Style-based approach </em>
<img src="figures/noise.png" alt="Characterization" height="700" width="700">
</p>



<template v-slot:right>

<p align="center">
<img src="figures/scheme2-1.png" alt="Characterization" height="300" width="300">
</p>

<p align="right">
<a href="https://arxiv.org/abs/2110.06933">2110.06933</a>
</p>

</template>

---

## Results with $pp \rightarrow t \bar{t}$

<p align="center">
<img src="figures/qgan.png" alt="Characterization" height="800" width="800">
<br>
<em> Implementation largely hardware independent! </em>

</p>

---
layout: center
class: text-center
---

# Outlook

---
layout: two-cols
---

We have presented Qibo, a simple full stack API capable of doing

<ul>
<li><p>High performance quantum simulation: <a href="https://github.com/qiboteam/qibojit">qibojit</a></p></li>
<li><p>Hardware control: <a href="https://github.com/qiboteam/qibolab">qibolab</a></p></li>
<li><p>Hardware calibration: <a href="https://github.com/qiboteam/qibocal">qibocal</a></p></li>
</ul>


Why should you choose Qibo?

<ul>
<li><p>Publicly available as an open source project</p></li>
<li><p>Modular layout design with the possibility of adding
<ul><!-- start of nested list-->
                <li><p>a new backend for simulation</p></li>
                <li><p>a new platform for hardware control</p></li>
            </ul><!--end of nested list--></p></li>
<li><p>Community driven effort</p></li>
<li><p>Many researchers are using it for HEP applications</p></li>

</ul>

<template v-slot:right>
<p align="center">
<img src="figures/GitHub1.png" alt="Characterization" height="800" width="800">
</p>
</template>


---
layout: center
class: text-center
---

# Thanks for listening!


---
layout: center
class: text-center
---

# Backup slides

---

# Fitting PDFs using adiabatic evolution
##

Use quantum adiabatic machine learning for the determination of PDF and sampling.

<div class="row">
  <div class="column">
    <div class="vertical-center">
    <img src="figures/adiabatic fit.png" alt="Snow" style="width:100%">
    </div>
  </div>
  <div class="column">
    <img src="figures/adiabatic fit2.png" alt="Snow" style="width:100%">

  </div>
</div>

<p align="right">
<a href="https://arxiv.org/abs/2303.11346">2303.11346</a>
</p>


---

# Paramater Shift Rule on Hardware
##

Successfully performed a gradient descent on a QPU with a single using Parameter Shift Rule algorithm.

<p align="center">
<img src="figures/ratio_cropped-1.png" alt="Characterization" height="700" width="700">
</p>

<p align="right">
<a href="https://arxiv.org/abs/2210.10787">2210.10787</a>
</p>



---
theme: apple-basic
class: text-center
highlighter: shiki
lineNumbers: false
drawings:
  persist: false
defaults:
  foo: true
transition: slide-left
title: Quantum simulation with just-in-time compilation
layout: intro

---

<div class="topright">
<figure>
    <img src="ACAT.jpg" width="80" alt="Bechmark" >
    </figure>
</div>

# Quantum simulation with just-in-time compilation

##  Andrea Pasquale, ACAT 2024



<br>
<br>
<br>
 <div class="container">
  <div class="column4">
    <figure>
    <img src="qibo.svg" width="80" alt="Bechmark" >
    </figure>

  </div>
  <div class="column4">
    <figure>
    <img src="unimi.png" width="120" alt="Bechmark" >
    </figure>

  </div>
  <div class="column4">
    <figure>
    <img src="infn.png" width="100" alt="Bechmark" >
    </figure>

  </div>
  <div class="column4">
    <figure>
    <img src="TII.jpg" width="100" alt="Bechmark" >
    </figure>

  </div>
</div>


---

## Why do we care about simulating quantum computers?

<br>

<ul>
<li> Classical simulation is fundamental to understand and design quantum hardware </li>
<li> To test and verify quantum algorithms before quantum hardware is ready </li>
</ul>


## However, simulation quantum computers is hard!

Common issues are:
<br>
<ul>
<li> Memory management </li>
<li> Optimize operation </li>
<li> Explore different architectures: CPU, GPU or GPUs.</li>
</ul>



Therefore, we need a framework that has some "tricks" to efficiently perform quantum simulation.

<div class="bottomright"> <a href="https://arxiv.org/pdf/2302.08880.pdf"> 2302.08880</a> </div>


---
layout: section
---
# Qibo
## A quantum computing framework for simulation and hardware execution

<div class="bottomright1"> <a href="https://github.com/qiboteam/qibo"> https://github.com/qiboteam/qibo </a> </div>


---
layout: image

image: qibo_ecosystem_webpage.svg
backgroundSize: contain
---
---
layout: section
---

# `Qibojit`
## Full state vector simulation with JIT

<div class="bottomright1"> <a href="https://github.com/qiboteam/qibojit"> https://github.com/qiboteam/qibojit </a> </div>

---

# Full-state vector simulation in a nutshell

<br>
N-qubits system is represented by 2N complex values.

<br>
<br>
Performing a quantum computing algorithms means to apply unitary operators
to the state-vector via matrix multiplication.
<br>

<br>
A good simulator corresponds to a good engine to perform
linear algebra operations.
<br>
<br>

## What can we do to improve performances?
<br>

<ul>
<li> Parallelize using multithread </li>
<li> Accelerate through GPUs </li>
<li> Depending on the language we might need to speed up the compilation process → JIT </li>
</ul>


---
layout: image-right
image: ./jit.png
---


# What is just-in-time compilation?

A method for improving the performance of interpreted programs.

We can exploit some frameworks in Python
to achieve better performances compared to the `numpy`:

<br m--8>

 <div class="container">
  <div class="column">
    <img src="numba.svg" alt="Snow" >
  </div>
  <div class="column">
    <img src="cupy.png" alt="Forest" >
  </div>
</div>



---
layout: image
image: ./Qibojit.png
backgroundSize: contain
---

<div class="bottomright"> <a href="https://quantum-journal.org/papers/q-2022-09-22-814/"> Quantum 6, 814 (2022)</a> </div>

---

 <div class="container">
  <div class="column">
    <figure>
    <img src="qibo_backends_dry.png" alt="Bechmark" >
    </figure>
  </div>
  <div class="column">
    <figure>
    <img src="qibo_backends.png" alt="Bechmark" >
    </figure>
  </div>
</div>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
    Total dry run (left) and simulation (right) time scaling with the number of qubits for simulating the qft circuit using
different Qibo backends.




---

<figure>
<img src="qj_dry_vs_sim.svg" alt="Bechmark" >
Comparison between import, dry run and simulation times for the three platforms of the qibojit backend.
</figure>


---

# Comparison with other libraries
<figure>
<img src="benchmark30q.png" alt="Bechmark" >
Total dry run time for simulating different circuits of 30 qubits, using libraries that
support single (top) and double (bottom) precision.
</figure>





---
layout: section
---
# Tensor Networks

## Approximate circuit execution

<div class="bottomright1"> <a href="https://github.com/qiboteam/qibotn"> https://github.com/qiboteam/qibotn </a> </div>

---
layout: two-cols
---

# Tensor Networks

<br>

Tensor Networks are a powerful method for simulating quantum circuits by representing the state or operator as a network of smaller tensors.
<br>
<br>

<figure>
    <img src="mps_diagram.png" alt="Bechmark" width="300" class="center" >
    </figure>

<br>
<br>


It is an approximate method that enable to simulate circuits with polynomial complexity.

::right::
<br>
<br>

<figure>
    <img src="tn_space.png" alt="Bechmark" width="300" class="center">
    <br>
    <figcaption> The expressive power of neural networks in quantum physics. Credits to G. Carleo.</figcaption>
  </figure>





---

# Speed-up comparison

Benchmarks executed on a NVIDIA A100 GPU with 40 GB

 <div class="container">
  <div class="column">
    <figure>
    <img src="tn_qft.png" alt="Bechmark" >
    </figure>
    <br>
    <br>
        <ul>
<li> qibojit-cupy faster</li>
<li> TN curves more flatten</li>
</ul>
  </div>
  <div class="column">
    <figure>
    <img src="tn_variational.png" alt="Bechmark" >
    </figure>
    <br>
    <br>
        <ul>
<li> MPS/MPI/NCCL all bear overheads </li>
<li> likely to pay off for larger qubits </li>
</ul>
  </div>
  <div class="column">
    <figure>
    <img src="tn_supremacy.png" alt="Bechmark" >
    </figure>
    <br>
    <br>
        <ul>
<li> curve incomplete for MPI/NCLL</li>
<li> cuQuantum pathfinding incurs huge memory usage</li>
</ul>
  </div>
</div>

---

# Tensor Network Scalability

<div h="full" flex="~ row" gap="lg" p="sm b-20">
<div flex="~ col" p="t-4">
<ul>
<li>1x A100 GPU achieved 400 qubits, likely to scale up even more
</li>
<br>
<br>
<li>TN curves are more flatten (good scalability), whereas state-vector curve is exponential (bad scalability)
</li>
</ul>
</div>

<div flex="~ col justify-center" v="full" p="t-10">
<figure>
<img src="tn_400.png" alt="TN">
</figure>

</div>
</div>

---
layout: section
---

# Clifford simulation

## Speeding up execution for Clifford circuits

<div class="bottomright1"> <a href="https://qibo.science/qibo/stable/api-reference/qibo.html#qibo.backends.clifford.CliffordBackend"> Qibo Clifford backend</a> </div>


---

# Clifford simulation in a nutshell

<br>

<div p="x-5 y-1" bg="gray-200 dark:gray-800">

**Theorem 1** *Given an n-qubit state $\ket{\psi}$, the following are
equivalent:*

<div m-l-3 text-sm>

(i) *$\ket{\psi}$ can be obtained from $\ket{0}\otimes n$ by CNOT,
Hadamard, and phase gates only.*<br>
(ii) *$\ket{\psi}$ can be obtained from $\ket{0}\otimes n$ by CNOT,
Hadamard, phase, and measurement gates only.*<br>
(iii) *$\ket{\psi}$ is stabilized by exactly 2n Pauli operators.*<br>
(iv) <span underline>***$\ket{\psi}$ is uniquely determined by $S (\ket{\psi}) = Stab
(\ket{\psi})\cap P_{n}$ or the group of Pauli operators that stabilize
$\ket{\psi}$***</span>

</div>
</div>

<br>

It can be shown <a href="https://journals.aps.org/pra/abstract/10.1103/PhysRevA.70.052328"> Phys. Rev. A 70, 052328 (2004) </a>
that the complexity is $O(n)$ for the application of a single gate while measurements can be performed
in $O(n^2)$.

<!-- ::right::
<div flex="~ col justify-center" text-sm>

$$
\left(
\begin{array}{ccc|ccc|c}
x_{11} & \dots & x_{1n} & z_{11} & \dots & z_{1n} & r_1 \\
\vdots & \ddots & \vdots & \vdots & \ddots & \vdots & \vdots \\
x_{n1} & \dots & x_{nn} & z_{n1} & \dots & z_{nn} & r_n \\
\hline
x_{(n+1)1} & \dots & x_{(n+1)n} & z_{(n+1)1} & \dots & z_{(n+1)n} & r_{n+1} \\
\vdots & \ddots & \vdots & \vdots & \ddots & \vdots & \vdots \\
x_{(2n)1} & \dots & x_{(2n)n} & z_{(2n)1} & \dots & z_{(2n)n} & r_{2n} \\
\end{array}
\right)
$$

Instead of operating on the whole state vector, the state is represented by a much more
compressed *tableau*.

It still requires vectorized operations on the boolean entries, that can be optimized in
a similar fashion to the general state vector approach.

</div> -->



---


<figure>
    <img src="clifford.png" alt="Snow" class="center">
    <figcaption>Simulation of clifford circuits with an increasing number of qubits (no measurements). For each point we take the average over 100 different randomly generated circuits. Each circuit is generated following <a href="https://arxiv.org/abs/2003.0941"> Quantum 6, 814 (2022)</a> , which guarantees an uniform distribution of the generated n-qubits clifford operators, but the depth is not fixed.</figcaption>
    </figure>


---

<figure>
    <img src="clifford_depths.png" alt="Snow">
    <figcaption>Simulation of clifford circuits with an increasing depth for a fixed number of qubits (no measurements). For each point we
    take the average over 100 different randomly generated circuits. We randomly sample circuit moments composed by a single qubit clifford layer
    followed by two-qubits cliffordo one. We sequentially stack up to 10000 layers constructed this way.</figcaption>
    </figure>



---
layout: section
---
# Outlook


---
layout: iframe-right
# the web page source
url: https://qibo.science

---

We have presented a fully open-source
quantum computing framework: `Qibo`

<span style="margin-left: 110px;"> <a href="https://qibo.science/"> https://qibo.science/ </a> </span>



`Qibo` is compatible with state-of-the-art quantum simulation
and offers several engines:

* `qibojit`: full-state vector simulation
* `qibotn`: tensor-network simulation through NVIDIA
* `clifford simulation`: for Clifford circuits

But `Qibo` is much more than a quantum simulator...

* Hardware execution and calibration → See talk [Edoardo's talk](https://indico.cern.ch/event/1330797/contributions/5796498/)
* Applications → See [Matteo's talk](https://indico.cern.ch/event/1330797/contributions/5796490/)



---
layout: section
---
# Thanks for listening!

---
---
layout: post
title:  "Floquet formalism"
---
<figure style="float: right; padding-left: 50px">
<img src="/assets/hammock.jpg" 
alt="img" width="240"/>
<figcaption>Getting a feel of periodic systems <br> in the balcony hammock.</figcaption>
</figure>

Similar to the last weeks, I have recently finished my work on another solver for the Schrödinger
and Master equation: the *Floquet solver*. Or more generally, the *Floquet formalism*.

The main difference to the Bloch-Redfield and the Linblad solver is the focus on periodic systems.
I.e. the formalism deals with periodic Hamiltonians. Many of the time-dependencies that are
considered in quantum mechanics are periodic. Hence, the formalism might be more efficient for those
problems.

I won't go into the mathematical details here, as they are explained in the
[QuTiP docs](https://qutip.org/docs/latest/guide/dynamics/dynamics-floquet.html).

<br> 
<br>
### Schrödinger equation in the Floquet formalism

Assume the periodicity of the Floquet formalism is *T*. The Floquet approach is then to decompose
the wavefunction into *Floquet modes*, functions that also have periodicity *T*, and *quasienergy
levels*. We can find these modes and quasienergies by diagonalizing the propagator of the system at
the initial time. From the initial state, we can obtain coefficients, which tell us how to combine
the Floquet modes to obtain the wavefunction. We can then propagate the Floquet modes in time (again
using the propagator) and combine them at to obtain the wavefunction.

Since the problem is periodic and no dissipation is present, it is enough to calculate the
wavefunction for the first period. The following periods have the same dynamics. In QuTiP there
exists a *lookup method*, which gives the results from the pre-calculated first period for any given
time. Hence, no calculation is needed after the first period was calculated.

In practice it is much more efficient to calculate the first period using `qutip.sesolve()` than
diagonalizing the propagator at many times. This is actually how the first period is calculated.
Probably, the Floquet methods in QuTiP could be optimized, but I guess it is enough to use the
highly optimized `qutip.sesolve()`.

### Master Equation in Floquet formalism

In the Master Equation we assume that the external driving field is periodic (again periodicity *T*)
. The interaction with the environment makes the Floquet formalism more tricky, because it is not
enough to precalculate the first period and then just copy the result from there.

The dissipative version of the Floquet formalism is derived in
[this paper](https://www.sciencedirect.com/science/article/abs/pii/S0370157398000222).
The paper gives a
compact form of the master equation for the systems' density matrix, which is then implemented in
QuTiP. I have added a small part of this derivation to the [QuTiP docs]() to show the exact
quantities implemented in QuTiP.

In this approach the coupling operators and the density matrix of the system are decomposed in the
Floquet basis of the system (without coupling). This allows to approximate the solution to the
Master equation of the system. Several other approximations (Markov-Born and Rotating Wave) are made
to derive a compact form of the master equation for the system. To me, the remarkable findings in
the solution are that the diagonal elements evolve completely separate from the non-diagonal
elements in the density matrix. This is for example different to the Bloch-Redfield solution for the
master equation.

Again, this method only works for weak system-bath coupling and if the dynamics of the system are
much slower than the dissipation dynamics (Markov approximation). However, I have found that the
Floquet approach seems to be more robust to higher drive frequencies than the Bloch-Redfield solver.
I couldn't find an explanation for that yet, but I guess it is due to the periodic approach of the
Floquet formalism.

### QuTiP Tutorials

I have added two notebooks to the
[`qutip-tutorials` repo](https://github.com/qutip/qutip-tutorials), which cover the Floquet
formalism.
One describes the usage of the solver functions `qutip.fsesolve()` and `qutip.fmmesolve()` which can
simply be applied to periodic problems, without an understanding of the Floquet formalism. The
second notebook covers the internals of the two functions and explains how the calculations are
made. If any user needs to modify this approach, this notebook will be helpful.
Both notebooks also run on the new version QuTiP 5.

As usual, I found some bugs along the way and worked on QuTiP in other ways, which I list below.

## Contributions to QuTiP since last post

* [PR-25](https://github.com/qutip/qutip-tutorials/pull/25) - Tutorials on Floquet formalism
* [PR-1958](https://github.com/qutip/qutip/pull/1958) - Description of dissipative Floquet formalism
  in QuTiP docs
* [PR-1952](https://github.com/qutip/qutip/pull/1952) - Add missing basis transformation to Floquet
  solver in QuTiP 5
* [PR-26](https://github.com/qutip/qutip-tutorials/pull/26) - Update to new nbmake version


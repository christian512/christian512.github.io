---
layout: post
title:  "Bloch-Redfield master equation solver"
---
<figure style="float: right; padding-left: 30px">
<img src="/assets/working_in_the_shade.jpeg" 
alt="img" width="240"/>
<figcaption>Using the backyard shade to <br> work on Bloch-Redfield tutorials</figcaption>
</figure>

The past week I worked on the *Bloch-Redfield master equation solver* that is implemented in QuTiP.
More specifically, I wanted to update the notebooks that are provided for this solver in the
[`qutip-notebooks`](https://github.com/qutip/qutip-notebooks) repository. These notebooks did not
work properly with the current release and did not contain much description. Hence, the update was
very much needed.

To be prepared for the upcoming release of QuTiP 5, I also ported these notebooks to work with that
version. Along this way I came across some missing features and some clarifications were needed in
the documentation.

<p style="margin-bottom:90px;"></p>


### Bloch-Redfield master equation

When it comes to solving a master equation with QuTiP, my standard approach (and probably for many
others too) is using the *Lindblad master equation solver* (`qutip.mesolve`). For this solver, the
interaction of the system with the environment (bath) is described by Lindblad operators, which are
called *collapse operators* in QuTiP. However, these operators do not originate from any underlying
physical model, but describe penomenological processes. In many situations these type of couplings
to the bath are enough to describe the enough. At least that was the case for my simulations in the
field of quantum optics. A description of this approach to solving a master equation can be found
[here](https://qutip.org/docs/latest/guide/dynamics/dynamics-master.html).

However, there are more complex setups where the system-bath coupling can not be described by
collapse operators. This is where the *Bloch-Redfield (BR) formalism* is beneficial. The system-bath
interaction is still described by coupling operators, but for each coupling term a
*noise-power-spectrum (NPS)* is required. Using the NPS the dissipation processes of the system are
described by the physical properties of the environment. The master equation in the BR formalism is
derived using a perturbative approach and assumes a weak system-bath interaction (similar to the
Lindblad approach). To solve the Bloch-Redfield master equation we need the following inputs:
the system's Hamiltonian, initial system state, coupling operators and the NPS for each coupling
operator. The functionality in QuTiP allows only hermitian coupling operators to simplify the
numerical implementation. More information on the Bloch-Redfield formalism and it's implementation
can be found in
the [docs](https://qutip.org/docs/latest/guide/dynamics/dynamics-bloch-redfield.html).

### Comparison of Bloch-Redfield and Linblad approach

Both approaches to the dissipative master equation share the approximation of weak interactions
between the system and the environment. The derivation of the Lindblad master equation guarantees
that the evolution of the density matrix is physical, i.e., preserves the trace and positivity. The
perturbative approach chosen in the Bloch-Redfield formalism does not guarantee a physical evolution
of the density matrix. Hence, the results should be checked for correctness and the deviation from a
unity trace should be monitored.

Consider a system that consists of two subsystems, e.g., an atom and a cavity. Both subsystems can
interact with the environment, i.e., there are operators for the atom interacting with the bath and
similar for the cavity. The atom and the cavity interact with each other. However, the Lindblad
approach implictly requires a weak interaction between the two subsystems, as the collapse operators
only act on eigenstates of one subsystem (and not the mixed ones). Hence, the energy differences
that occur due to the collapse operators are not included. If the atom-cavity coupling
becomes too strong, the Lindblad approach could produce wrong results.

The situation of strongly coupled subsystems is where the Bloch-Redfield approach has advantages.
The description by the NPS allows any subsystem coupling strength, as it takes the energy difference
between eigenstates of the full system into account.

### QuTiP Tutorials

I have modified/written four notebooks to guide users with the usage of `brmesolve()`, which
implements the master equation solver using the Bloch-Redfield approach. You can find these
notebooks soon in the [`qutip-tutorials` repo](https://github.com/qutip/qutip-tutorials).

The first notebook summarizes the basic usage of `brmesolve()` with an example of a two-level
system. It also introduces the `bloch_redfield_tensor()` function. This tensor is calculated within
the Bloch-Redfield formalism and can be used to calculate the steadystate of a system with
dissipation. The second notebook uses the Jaynes-Cummings model to compare results
from `brmesolve()` with the results of the Lindblad solver `mesolve()`. We can see the differences
for strong atom-cavity coupling, as I have described above.
The next notebook deals with `brmesolve()` and time-dependent Hamiltonians and time-dependent
dissipation. The time-dependece requires some different function definitions, which are explained
there.
The last notebook already existed in the `qutip-notebooks` repository and describes a paper where
the dissipation from a quantum dot to phonons is described by the Bloch-Redfield formalism. With the
information from the previous three notebooks it should be easy to understand the QuTiP functions
used in this notebook. From all four notebooks this is the most advanced.

Furthermore, I created a version of each notebook that runs with the upcoming release of QuTiP 5.
Some functions changed slightly and I had to update the documentation accordingly. With these
notebooks, I think that `brmesolve()` should be ready for the release.

## Contributions to QuTiP since last post

* [PR-24](https://github.com/qutip/qutip-tutorials/pull/24) - Adding brmesolve notebooks
* [PR-1950](https://github.com/qutip/qutip/pull/1950)- Update documentation for brmesolve
* [ISSUE-1949](https://github.com/qutip/qutip/issues/1949)- Discussion on how to make transition to
  v5 more smooth.
* [PR-1951](https://github.com/qutip/qutip/pull/1951)- Support for different spectra types in
  bloch_redfield_tensor

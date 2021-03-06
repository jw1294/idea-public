HF (Hartree--Fock)
==================


The HF code solves the Hartree--Fock equation to approximately 
calculate the ground state density of a one-dimensional finite system 
of :math:`N` like-spin electrons, with Hamiltonian 
:math:`\hat{T} + \hat{U} + \hat{V}_{\text{ext}}`, where 
:math:`\hat{U}` is the operator for the softened Coulomb interaction 
potential given by 

.. math:: u(x, y) = (1 + |x-y|)^{-1} \, .

A perturbing potential may be applied to the ground state system, and 
the time-dependent Hartree--Fock equation solved approximately to 
calculate the system's time evolution.

Calculating the ground state
----------------------------

Orbitals and corresponding non-degenerate 
energy eigenvalues :math:`\{ \varphi_{j}, \varepsilon_{j} \}_{j=1}^{N}` 
for a non-interacting system of :math:`N` electrons are first 
calculated from the single-particle Schrödinger equation with 
Hamiltonian :math:`\hat{T} + \hat{V}_{\text{ext}}`.

We then proceed to calculate the ground state electron density of this 
system:

.. math:: n(x) = \sum_{j=1}^{N} \lvert \varphi_{j}(x) \rvert ^{2} \, .

From this we find the Hartree potential

.. math:: v_{\text{H}}(x) = \int \! n(y)u(x,y) \, \mathrm{d}y \, ,

and the Fock matrix (the self-energy in the exchange-only 
approximation)

.. math:: \Sigma_{\text{x}}(x,y) = - \sum_{j=1}^{N} \varphi_{j}^{*}(y) \varphi_{j}(x) u(x,y) \, .

Defining

.. math:: H(x,y) = \delta(x-y)\hat{T} + \delta(x-y)v_{\text{ext}}(y) + \delta(x-y)v_{\text{H}}(y) + \Sigma_{\text{x}}(x,y) \, ,

the Hartree--Fock Hamiltonian :math:`\hat{H}` acts via the integral 
transform

.. math:: \hat{H}\varphi(x) = \int \! H(x,y)\varphi(y) \, \mathrm{d}y \, .

When our one-dimensional space is discretized to points on a grid, 
:math:`H(x,y)` itself is a two-dimensional array, and acts as the 
Hamiltonian on the now one-dimensional arrays 
:math:`\{ \varphi_{j} \}_{j=1}^{N}`.

We solve the Hartree--Fock equation

.. math:: \hat{H}\varphi_{j} = \varepsilon_{j}\varphi_{j}

to obtain a new set of orbitals and their corresponding eigenvalues.

Using these new orbitals the above process is iterated until a 
self-consistent solution is reached (i.e. until the change in the new 
electron density :math:`n` is small enough compared to that of the 
previous iteration).


Time-dependence
---------------

A perturbing potential :math:`v_{\text{ptrb}}` is added to the 
ground state system so that it now has Hamiltonian 
:math:`\hat{T} + \hat{U} + \hat{V}_{\text{ext}} + \hat{V}_{\text{ptrb}}`.

We construct the Time-Dependent Hartree--Fock Hamiltonian, which again 
acts via the same integral transform as explained previously, but now 
with a perturbation potential term added as

.. math:: H^{\text{TD}}(x,y) = H(x,y) + \delta(x-y)v_{\text{ptrb}}(y) \, .

Beginning with the ground state (initial time) orbitals and energies 
:math:`\{ \varphi_{j}(t=0), \varepsilon_{j}(t=0) \}_{j=1}^{N}` as 
already calculated, we apply the Crank--Nicolson method to evolve the 
system forward in time by a step of size :math:`\delta t`:

.. math:: \{ \varphi_{j}(t) \}_{j=1}^{N} \rightarrow \{ \varphi_{j}(t=t+\delta t) \}_{j=1}^{N} \, .

This process is repeated until the desired duration of the 
time-evolution has been simulated. The electron density of the system 
is calculated from the orbitals at each time-step.

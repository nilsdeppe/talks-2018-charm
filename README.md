This is my talk that I'm giving at the [2018 Charm++
workshop](https://charm.cs.illinois.edu/workshops/charmWorkshop2018/).

**Title:**

A SpECTRE With a New 'face

**Abstract:**

Advanced LIGO has begun the exciting new era of gravitational wave astronomy
with its groundbreaking discoveries
of gravitational waves (GWs) from the merger of two black holes (BHBH).  In
addition to BHBH mergers, LIGO has observed binary neutron star (NSNS) mergers
with telescopes observing the merger event in the electromagnetic spectrum. This
means that NSNS mergers are multi-messengers, they can be detected using
gravitational wave detectors, and telescopes. The gravitational and
electromagnetic waves emitted by NSNS mergers do not just depend on the initial
masses of the stars, but also on the microphysics of them. For example, the way
neutrinos interact and are emitted, or the equation of state inside the stars
play a crucial role in both the gravitational and electromagnetic waves
emitted. Unfortunately, the treatment of neutrinos and the equation of state are
both things that are not yet very well understood. The cores of neutron stars
are well above nuclear density and so no experiments on Earth have been able to
tell us anything about physics at such scales. One of the major goals of
studying NSNS mergers is to understand physics at these extremely high
densities. Currently, however, state-of-the-art simulations do not have enough
accuracy to be able to constrain models very tightly. Specifically, the
computational errors are in the range of 1--10% and often not even quantifiable
with current algorithmic and hardware limitations.  Also, the simulations take
too long: several months on present supercomputers even at the current low
accuracy. Moreover, the methods do not scale well to upcoming exascale
machines. Our code *SpECTRE* ([first paper](https://arxiv.org/abs/1609.00098))
is designed from the ground
up to solve problems our current codes struggle to, with focus on petascale and
exascale platforms. It features two key new methods that will make breakthrough
simulations of neutron mergers and core-collapse supernovae possible:
Discontinuous Galerkin (DG) discretization and task-based parallelism.

Over the past year we have ported and cleaned up the majority of the code we
previously had in our private repository and *SpECTRE* is now available
at https://github.com/sxs-collaboration/spectre.  We have implemented a
variety of new features focusing on efficiency, code clarity and
ease-of-use. One of the major new features is the computational domain
decomposition. The domain is divided into blocks, which are deformed hexahedra
and make the up coarsest refinement level of the computational domain for our
under-development adaptive mesh refinement implementation. Currently spheres,
shells, wedges, and a general trilinear transformation are implemented, as well
as several other shapes required for performing simulations of compact object
binaries.

In order to successfully simulate black hole binaries a moving grid, dual-frame,
or Arbitrary-Lagrangian-Eulerian (ALE) approach must be used. Specifically, the
partial differential equations are solved in what we call the "grid" frame,
which is connected to the "intertial" frame (the physical frame) by a series
of time-dependent coordinate transformations. The net result is that the
excision spheres around the black holes rotate, move inwards, and deform with
the black holes. A dual-frame approach has been found to be required for
high-accuracy binary black hole evolutions using spectral-type methods such as
DG. We have implemented some of the basic time-dependent coordinate maps as well
as functions of time that are the parameters of the maps, and are starting
development on the control system that will dynamically adjust the
time-dependent coordinate maps.  The time-dependent coordinate maps must track
the black holes, which requires us to be able to find them. A basic horizon
finder to find black holes is implemented, and work on advanced features such as
adaptive refinement of the horizon surfaces is being developed.

Another major new feature that has been implemented is a novel high-order
conservative local time stepping algorithm. The ability to do local time
stepping is expected to be especially useful with adaptive mesh refinement,
since astrophysical simulations often range many orders of magnitude in
spatiotemporal scales.

Design decisions while moving *SpECTRE* to GitHub have been focusing on
making the code easier to use for physicists. To this end we developed a layer
on top of *Charm++* that eliminates the need for *Charm++*'s
interface files. The motivation for removing interface files is that we have
found them to be difficult to write and lead to even more difficult to debug
runtime errors, especially when writing generic code. Our interface eliminates
many of these pitfalls. Any pitfalls that are not eliminated trigger static
assertions to provide compile time error messages to the developer. We perform
automatic registration of chares as well as all entry methods invoked, even
entry method templates, which have in the past been the source of difficult to
find bugs.

I will briefly review the motivation behind *SpECTRE*, outline what we
have accomplished, and then present our interface to *Charm++* and how it
could be used to replace interface files in the next generation of
*Charm++*.

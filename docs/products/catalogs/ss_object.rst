.. _catalogs-ss-object:

#########
SS object
#########

.. important::

   The Solar System tables are available via the TAP service as of Nov 30 2026.


Derived parameters for moving (Solar System) objects accepted by the Minor Planet Center (MPC).

Schema: `PPDB SSObject table <https://sdm-schemas.lsst.io/ppdb.html#SSObject>`_


Access
======

As of Nov 30 2026, the SS tables are accessible via the TAP service.
In early 2027, these tables will become accessible via the Butler (see the `timeline for early science <https://rubinobservatory.org/for-scientists/resources/early-science>`_).

TAP
---

* |ss_object_doi|
* Table name: ``SSObject``


Description
===========

In these tables, a "Solar System object" (``SSObject``) is a small body known to the Minor Planet Center (MPC) with at least one Rubin observation accepted by the MPC, whether Rubin discovered it or observed a previously known object.

``SSObject`` includes one row per object with at least one MPC-accepted Rubin observation, built only from those observations (``SSObservation``), with observation summaries, per-band absolute-magnitude fits, and selected orbit-derived quantities.
The ``SSObject`` table includes the integer identifier ``ssObjectId`` (an opaque identifier that changes if the MPC changes the object's primary provisional designation; see :ref:`moving`), the unpacked primary provisional ``designation``, total and per-band observation counts, the first observation epoch and observing arc, per-band phase-angle ranges, extendedness summaries, and selected quantities derived from the input MPC orbit (``mpc_orbits``).

The ``SSObject`` table can be joined to the :doc:`SSObservation <ss_observation>` and :doc:`NearbySSO <nearby_sso>` tables on the ``ssObjectId`` or ``designation`` to obtain Minor Planet Center (MPC)-submitted observations (``SSObservation``) and nearest predicted known Solar System object per ``DiaSource`` (``NearbySSO``).

The per-band absolute magnitudes are fitted to the PSF photometry of the object's ``SSObservation`` rows with the H-G12 phase function.
A 0.05 magnitude uncertainty floor is added in quadrature; a robust fit first identifies outliers beyond 10 sigma, which are excluded from the final least-squares fit.
H and G12 are fitted together, with G12 constrained to [0, 1].
Where the slope cannot be constrained (fewer than three usable points, a phase-angle span under 5 degrees, a fit ending at the edge of G12's range, or a singular fit), ``{band}_slope_fit_failed`` is true, G12 is set to 0.5, H is fitted with G12 fixed, and ``{band}_G12Err`` and the H-G12 covariance are NULL.
Users should check ``{band}_slope_fit_failed``, compare the per-band ``nObsUsed`` and ``nObs`` values, inspect the phase-angle coverage (``{band}_phaseAngleMin`` and ``{band}_phaseAngleMax``) and fit statistics, and treat sparsely sampled fits and cross-band colors with care.

The table also contains the Tisserand parameter with respect to Jupiter and Earth minimum-orbit-intersection-distance quantities computed from the input MPC elements.
These values should not by themselves be used for hazard classification or close-approach prediction.

The ``SSObject`` catalog is rebuilt daily from the ``SSObservation`` table, i.e., from the observations Rubin has submitted to and the MPC has accepted.

Processing
----------

The SS object catalog is the result of :doc:`/processing/moving/index`.

Tutorials
---------

See the 200-level catalog :doc:`/tutorials/index` for a notebook on the SS object table.
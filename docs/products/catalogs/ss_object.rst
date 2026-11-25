.. _catalogs-ss-object:

#########
SS object
#########

.. important::

   The Solar System tables are available via the TAP service as of Nov 30 2026.


Per-object properties of the Solar System objects with Rubin observations accepted by the Minor Planet Center (MPC).

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

``SSObject`` has one row per Solar System object with at least one Rubin observation accepted by the Minor Planet Center (MPC), whether Rubin discovered the object or observed a known one.
Its values are computed from those observations (the object's ``SSObservation`` rows) and from the object's MPC orbit.

The table contains:

- ``ssObjectId`` and ``designation`` (the unpacked primary provisional designation).
  ``ssObjectId`` is an opaque identifier: it changes if the MPC changes the object's primary designation (see :ref:`moving`).
- observation counts, in total and per band, the time of the first observation, and the observed arc;
- per-band phase-curve fits (below) and phase-angle ranges;
- extendedness summaries (``extendednessMin``, ``extendednessMedian``, ``extendednessMax``);
- quantities computed from the MPC orbit: the Earth minimum orbit intersection distance (MOID) and the Tisserand parameter with respect to Jupiter.
  On their own, these should not be used for hazard assessment or close-approach prediction.

Phase-curve fits
----------------

In each band, H and G12 of the H-G12 phase function are fitted to the PSF photometry, with G12 limited to the range 0 to 1.
A 0.05 mag uncertainty floor is added in quadrature.
A robust fit first identifies outliers beyond 10 sigma, which are left out of the final least-squares fit.

If the slope cannot be constrained, ``{band}_slope_fit_failed`` is true.
G12 is then fixed at 0.5 and only H is fitted; ``{band}_G12Err`` and the H-G12 covariance are NULL.
This happens when:

- fewer than three points are usable;
- the points span less than 5 degrees of phase angle;
- the fitted G12 lands at 0 or 1, or at 0.2, where the G12 function changes form; or
- the fit is singular.

Before using a fit, check ``{band}_slope_fit_failed``, compare ``{band}_nObsUsed`` with ``{band}_nObs``, and look at the phase-angle coverage (``{band}_phaseAngleMin``, ``{band}_phaseAngleMax``) and ``{band}_Chi2``.
Treat sparsely sampled fits, and colors derived from them, with care.

Joins
-----

- To :doc:`SSObservation <ss_observation>` on ``ssObjectId`` or ``designation``, for the individual observations.
- To :doc:`mpc_orbits <mpc_orbits>` on ``designation``, for the orbit.
- To :doc:`NearbySSO <nearby_sso>` on ``ssObjectId`` or ``designation``.

Updates
-------

``SSObject`` is rebuilt daily from ``SSObservation``.


Processing
----------

The SS object catalog is the result of :doc:`/processing/moving/index`.

Tutorials
---------

See the 200-level catalog :doc:`/tutorials/index` for a notebook on the SS object table.
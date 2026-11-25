.. _catalogs-ss-object:

#########
SS object
#########

.. important::

   Prompt catalogs are not yet available.


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

A "Solar System object" is a moving object for which one or more signal-to-noise (SNR) ratio >5 difference image detections (``DIASources``) or direct image detections (``Sources``) have been linked together either as an association to a previously known small body or as a new discovery and has been accepted by the Minor Planet Center (MPC).

``SSObject`` includes one row per recovered known object built only from the MPC-accepted observations (``SSObservations``), with observation summaries, per-band absolute-magnitude fits, and selected orbit-derived quantities.
The ``SSObject`` table includes the unique identifier integer ``ssObjectId``, the unpacked primary provisional ``designation``, total and per-band observation counts, the first observation epoch and observing arc, per-band phase-angle ranges, extendedness summaries, and selected quantities derived from the input MPC orbit (``mpc_orbits``).

The ``SSObject`` table can be joined to the :doc:`SSObservation <ss_observation>` and :doc:`NearbySSO <nearby_sso>` tables on the ``ssObjectId`` or ``designation`` to obtain Minor Planet Center (MPC)-submitted observations (``SSObservation``) and nearest predicted known Solar System object per ``DiaSource`` (``NearbySSO``).

The ``SSObject`` table per-band absolute magnitudes were fitted to the associated PSF photometry with an H-G12 phase function.
G12 was fixed at 0.5 and only H was fitted; valid G12 values therefore all equal 0.5, while G12 uncertainties and the H-G12 covariance are undefined.
A 0.05 magnitude uncertainty floor was included, and a robust initial fit was used to reject measurements beyond 10 sigma before the final least-squares fit.
Users should compare the per-band ``nObsUsed`` and ``nObs`` values, inspect the phase-angle coverage and fit statistics, and treat sparsely sampled fits and cross-band colors with care.

The table also contains the Tisserand parameter with respect to Jupiter and Earth minimum-orbit-intersection-distance quantities computed from the input MPC elements.
These values should not by themselves be used for hazard classification or close-approach prediction.

The PPDB contains all alerts from roughly September 2025 onwards as well as the accumulated Solar System tables. The ``SSObject`` catalog is rebuilt daily from the moving object observations Rubin submits to and are accepted by the MPC.

Processing
----------

The SS object catalog is the result of :doc:`/processing/moving/index`.

Tutorials
---------

See the 200-level catalog :doc:`/tutorials/index` for a notebook on the SS object table.
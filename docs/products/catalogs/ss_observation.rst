.. _catalogs-ss-observation:

##############
SS observation
##############

.. important::

   Prompt catalogs are not yet available.


MPC record of each Rubin-submitted and MPC-accpeted observation for moving (Solar System) objects, independent of the PPDB ``DiaSource`` table.

* Schema: `PPDB SS Observation table <https://sdm-schemas.lsst.io/ppdb.html#SSObservation>`_

Access
======

As of Nov 30 2026, the SS tables are accessible via the TAP service.
In early 2027, these tables will become accessible via the Butler (see the `timeline for early science <https://rubinobservatory.org/for-scientists/resources/early-science>`_).

TAP
---

* |ss_observation_doi|
* Table name: ``SSObservation``


Description
===========

A "Solar System object" is a moving object for which one or more signal-to-noise (SNR) ratio >5 difference image detections (``DIASources``) or direct image detections (``Sources``) have been linked together either as an association to a previously known small body or as a new discovery and has been accepted by the Minor Planet Center (MPC).

``SSObservation`` includes one row per Minor Planet Center (MPC) record (obs_sbn) of each Rubin-submitted and MPC-accpeted observation of a moving object, joined to the full measurement from which it was submitted (a difference image ``DiaSource`` or a direct image ``Source``) and its ephemeris from ``mpc_orbits``.
The ``SSObservation`` table is independent of the PPDB ``DiaSource`` table.
``SSObservation`` contains the ``DiaSource``- or ``Source``-like columns for the difference image detection or direct image detection from which the Rubin observation was submitted to the MPC; there should thus never be a need to join the ``SSObservation`` and ``DiaSource`` tables.
In addition to measured astrometry and photometry submitted to the MPC, ``SSObservation`` also includes ephemeris quantities predicted from ``mpc_orbits``.

The ``obsid``, ``obssubid``, ``trksub``, ``trkid``, ``submission_id``, and ``status`` fields in the ``SSObservation`` table provide identifying information about the MPC observation record.
The ``measuredOn`` field tells the kind of image the row (observation) was measured on: 'difference' for ``DiaSource`` rows, 'science' for ``Source`` rows (detections).
The ``processing``, ``processingTable``, and ``diaSourceId``/``sourceId`` fields with their parent ids provide submitted measurement information.
In addition, the ``Source``-only 3/6/25 pixel apertures and magnitudes are also included.
The ``ssObjectId`` unique identifier is NULL when the observation has no ``SSObject`` (e.g., for 'Isolated Tracklet File' rows); the ``designation`` unique identifier is also included in the ``SSObservation`` table.
``midpointMjdTai`` is corrected for the shutter's motion; ``midpointMjdTai_flag`` notes where the midpoint isn not corrected for shutter timing.
The ephemeris and geometry information includes the predicted position's error ellipse (``ephRaErr``, ``ephDecErr``, ``ephRa_ephDec_Cov``) and the tail position angles (``ephAntiSunPA``, ``ephAntiMotionPA``).

The ``SSObservation`` table can be joined to the :doc:`SSObject <ss_object>` and :doc:`NearbySSO <nearby_sso>` tables on the ``ssObjectId`` or ``designation`` to obtain Rubin-derived object-level photometry and summary properties (``SSObject``) and nearest predicted known Solar System object per ``DiaSource`` (``NearbySSO``).

The PPDB contains all alerts from roughly September 2025 onwards as well as the accumulated Solar System tables. The ``SSObservation`` catalog is rebuilt daily from the moving object observations Rubin submits to and are accepted by the MPC.


Processing
----------

The SS observation catalog is the result of :doc:`/processing/moving/index`.

Tutorials
---------

See the 200-level catalog :doc:`/tutorials/index` for a notebook on the SS observation table.
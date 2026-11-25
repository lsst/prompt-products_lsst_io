.. _catalogs-ss-observation:

##############
SS observation
##############

.. important::

   The Solar System tables are available via the TAP service as of Nov 30 2026.


MPC record of each Rubin-submitted and MPC-accepted observation of a moving (Solar System) object, with the measurement it was submitted from, independent of the PPDB ``DiaSource`` table.

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

In these tables, a "Solar System object" (``SSObject``) is a small body known to the Minor Planet Center (MPC) with at least one Rubin observation accepted by the MPC, whether Rubin discovered it or observed a previously known object.

``SSObservation`` includes one row per Minor Planet Center (MPC) record (obs_sbn) of each Rubin-submitted and MPC-accepted observation of a moving object, joined to the full measurement from which it was submitted (a difference image ``DiaSource`` or a direct image ``Source``) and its ephemeris from ``mpc_orbits``.
The ``SSObservation`` table is independent of the PPDB ``DiaSource`` table.
``SSObservation`` contains the ``DiaSource``- or ``Source``-like columns for the difference image detection or direct image detection from which the Rubin observation was submitted to the MPC; there should thus never be a need to join the ``SSObservation`` and ``DiaSource`` tables.
In addition to measured astrometry and photometry submitted to the MPC, ``SSObservation`` also includes ephemeris quantities predicted from ``mpc_orbits``.

The ``obsid``, ``obssubid``, ``trksub``, ``trkid``, ``submission_id``, and ``status`` fields in the ``SSObservation`` table provide identifying information about the MPC observation record.
The ``measuredOn`` field tells the kind of image the row (observation) was measured on: 'difference' for ``DiaSource`` rows, 'science' for ``Source`` rows (detections).
The ``processing``, ``processingTable``, and ``diaSourceId``/``sourceId`` fields with their parent ids provide submitted measurement information.
In addition, the ``Source``-only 3/6/25 pixel apertures and magnitudes are also included.
The ``ssObjectId`` is NULL when the observation is not identified with an object that has an ``SSObject`` row: observations in the MPC's Isolated Tracklet File (``status`` = 'I': accepted, but not identified with any known object) and, rarely, designated objects missing from ``mpc_orbits``. The ``designation`` (the object's unpacked primary provisional designation) is NULL for Isolated Tracklet File observations.
``midpointMjdTai`` is corrected for the shutter's motion; ``midpointMjdTai_flag`` is true where no correction is available and the midpoint is the visit's.
The ephemeris and geometry information includes the predicted position's error ellipse (``ephRaErr``, ``ephDecErr``, ``ephRa_ephDec_Cov``) and the tail position angles (``ephAntiSunPA``, ``ephAntiMotionPA``).

The ``SSObservation`` table can be joined to the :doc:`SSObject <ss_object>` and :doc:`NearbySSO <nearby_sso>` tables on the ``ssObjectId`` or ``designation`` to obtain Rubin-derived object-level photometry and summary properties (``SSObject``) and nearest predicted known Solar System object per ``DiaSource`` (``NearbySSO``).

The ``SSObservation`` catalog is rebuilt daily from the observations Rubin has submitted to and the MPC has accepted. These include observations from reprocessings of earlier data (e.g., DP1 and DP2; see ``processing``) as well as from alert production, so the table covers a longer period than the alerts in the PPDB. Because an observation appears only once the MPC has accepted and published it, ``SSObservation`` lags the alert stream.


Processing
----------

The SS observation catalog is the result of :doc:`/processing/moving/index`.

Tutorials
---------

See the 200-level catalog :doc:`/tutorials/index` for a notebook on the SS observation table.
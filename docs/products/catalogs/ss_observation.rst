.. _catalogs-ss-observation:

##############
SS observation
##############

.. important::

   The Solar System tables are available via the TAP service as of Nov 30 2026.


Rubin's observations of Solar System objects accepted by the Minor Planet Center (MPC), each with the measurement it was submitted from.

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

``SSObservation`` holds Rubin's observations of Solar System objects that the Minor Planet Center (MPC) has accepted, one row per accepted observation (an MPC ``obs_sbn`` record).
Each row combines:

- the MPC's record of the observation: its identifiers and status;
- the full Rubin measurement it was submitted from, either a difference-image ``DiaSource`` or a direct-image ``Source``;
- the object's predicted position and observing geometry, computed from ``mpc_orbits``.

Because each row already contains the full measurement, there is no need to join ``SSObservation`` to the ``DiaSource`` table.
Many rows have no counterpart there anyway.
Besides alert production, the table includes observations from reprocessed earlier data (e.g., DP1 and DP2), so it covers a longer period than the PPDB's alerts.

Key columns
-----------

- ``obsid``, ``obssubid``, ``trksub``, ``trkid``, ``submission_id``, ``status``: the MPC's record of the observation.
- ``measuredOn``: the kind of image measured, 'difference' (a ``DiaSource``) or 'science' (a ``Source``).
- ``processing`` and ``processingTable``: which processing the measurement comes from (e.g., ``AP-DS`` for alert production, ``DP2-DS`` for the DP2 reprocessing).
  ``diaSourceId`` or ``sourceId`` identifies the measurement within that processing.
  Only ``AP-DS`` measurements are in the PPDB's ``DiaSource`` table, and the row already contains the full measurement anyway (see :ref:`moving`).
- ``ap03Flux``, ``ap06Flux``, ``ap25Flux`` (and their magnitudes): 3, 6, and 25 pixel aperture photometry, for ``Source`` rows only.
- ``ssObjectId`` and ``designation``: the object the MPC identified the observation with.
  Both are NULL for observations in the MPC's Isolated Tracklet File (``status`` = 'I'): accepted, but not yet identified with a known object.
  Rarely, ``ssObjectId`` is also NULL for an object missing from ``mpc_orbits``.
- ``midpointMjdTai``: the exposure midpoint at the source's position, corrected for the shutter's motion.
  ``midpointMjdTai_flag`` is true where no correction is available and the visit's midpoint is used instead.
- ``ephRa``, ``ephDec``, and the other ``eph`` columns: the predicted position, its uncertainty (``ephRaErr``, ``ephDecErr``, ``ephRa_ephDec_Cov``), and the position angles of the anti-Sun and anti-motion directions (``ephAntiSunPA``, ``ephAntiMotionPA``), useful for tails.

Joins
-----

- To :doc:`SSObject <ss_object>` on ``ssObjectId`` or ``designation``, for per-object properties.
- To :doc:`mpc_orbits <mpc_orbits>` on ``designation``, for the orbit.
- To :doc:`NearbySSO <nearby_sso>` on ``ssObjectId`` or ``designation``.

Updates
-------

``SSObservation`` is rebuilt daily.
An observation appears only after the MPC has accepted and published it, so the table lags the alert stream.


Processing
----------

The SS observation catalog is the result of :doc:`/processing/moving/index`.

Tutorials
---------

See the 200-level catalog :doc:`/tutorials/index` for a notebook on the SS observation table.
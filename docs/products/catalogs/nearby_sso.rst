.. _catalogs-nearby-sso:

##########
Nearby SSO
##########

.. important::

   The Solar System tables are available via the TAP service as of Nov 30 2026.


Known Solar System objects predicted near each ``DiaSource``, and predictions that matched no ``DiaSource``.

* Schema: `PPDB Nearby SSO table <https://sdm-schemas.lsst.io/ppdb.html#NearbySSO>`_


Access
======

As of Nov 30 2026, the SS tables are accessible via the TAP service.
In early 2027, these tables will become accessible via the Butler (see the `timeline for early science <https://rubinobservatory.org/for-scientists/resources/early-science>`_).

TAP
---

* |nearby_sso_doi|
* Table name: ``NearbySSO``


Description
===========

``NearbySSO`` lists the known Solar System objects predicted to be near Rubin's difference-image detections.
It answers "which known object could this ``DiaSource`` be?", not "is this ``DiaSource`` that object?":
a row records proximity, not an identification.
Confirmed observations are in :doc:`SSObservation <ss_observation>`.

The table has two kinds of rows:

- **Matched predictions** (``diaSourceId`` set): for each ``DiaSource``, the nearest known object predicted within 5 arcseconds (15 arcseconds for comets and interstellar objects).
- **Unmatched predictions** (``diaSourceId`` NULL): known objects predicted to fall on a detector of the visit, or within 30 arcseconds of one, that matched no ``DiaSource``.
  Use these to find non-detections.

Predictions are computed from the MPC orbits (``mpc_orbits``).
Only objects whose predicted position is known to better than 10 arcseconds (1-sigma) are included.

Unlike ``SSObservation`` and ``SSObject``, which contain only MPC-accepted observations, ``NearbySSO`` covers every ``DiaSource`` in the PPDB.

Key columns
-----------

- ``designation``: the predicted object's unpacked primary provisional designation. Always present.
- ``ssObjectId``: the object's identifier in ``SSObject``. NULL if the MPC has not yet accepted any Rubin observation of the object.
- ``ephRa``, ``ephDec``, ``ephVmag``: the predicted position and V magnitude.
  ``ephRaErr``, ``ephDecErr``, and ``ephRa_ephDec_Cov`` give the uncertainty of the position.
- ``ephOffset``: the distance between the ``DiaSource`` and the predicted position.
  Compare it with the position's uncertainty to judge how likely the match is.
- ``diaDistanceRank``: when several ``DiaSources`` of a visit lie within the matching radius of one prediction, this one's rank by distance (1 for the nearest; ties go to the lower ``diaSourceId``).
  A rank above 1 means a closer ``DiaSource`` exists, which may belong to another object.
  NULL for unmatched predictions.

Joins
-----

- To :doc:`DiaSource <dia_source>` on ``diaSourceId`` (matched predictions only).
- To :doc:`SSObject <ss_object>` and :doc:`SSObservation <ss_observation>` on ``ssObjectId`` or ``designation``.

Updates
-------

``NearbySSO`` is rebuilt daily from all PPDB ``DiaSources``, using the current MPC orbits and designations.
It therefore picks up new discoveries, improved orbits, and designation changes.
Each prediction is computed for its row's ``midpointMjdTai``, which is corrected for the shutter's motion where a correction is available.


Processing
----------

The NearbySSO catalog is the result of :doc:`/processing/moving/index`.

Tutorials
---------

See the 200-level catalog :doc:`/tutorials/index` for a notebook on the Nearby SSO table.
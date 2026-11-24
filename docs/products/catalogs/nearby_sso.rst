.. _catalogs-nearby-sso:

##########
Nearby SSO
##########

.. important::

   Prompt catalogs are not yet available.


Nearest predicted known Solar System object for each ``DiaSource``, including the nearest objects predicted to land on a detector but match no ``DiaSource`` (with ``DiaSourceId`` = NULL).

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

One row per nearest predicted known Solar System object for each ``DiaSource``, including the nearest objects predicted to land on a detector but match no ``DiaSource`` (with ``DiaSourceId`` = NULL).

The ``NearbySSO`` table includes the unique identifiers ``diaSourceId``, ``ssObjectId``, and ``designation``.
It also contains the ephemeris information (``ephRa``, ``ephDec``, and ``ephVmag``) with the predicted position's error ellipse (``ephRaErr``, ``ephDecErr``, ``ephRa_ephDec_Cov``).
``ephOffset`` provides the total observed versus predicted angular separation on the sky between the observed ``DiaSource`` and the predicted position of a known solar system object.

Because a single known moving object predicted position may fall within the 1-arcsecond association radius for multiple nearby ``DiaSources``, a ``diaDistanceRank`` field is included in the ``NearbySSO`` table with the rank of the ``diaSourceId`` -identified source in terms of its closeness to the predicted SSO position, among all ``DiaSources`` of the same visit within the object's matching radius of that prediction.
If the ``diaSourceId`` is the nearest ``DiaSource`` to this SSO prediction, ``diaDistanceRank`` = 1 would be set.
If it is the second nearest, it would be 2, etc, with ties broken by the lower ``diaSourceId``.
A rank above 1 means a closer ``DiaSource`` exists, which may be attributed to another object.
``diaDistanceRank`` is NULL for a prediction that matched no ``DiaSource``.

To explore how many objects have been positionally matched to ``DiaSource`` detections within a radius of some number of arcseconds, query ``NearbySSO``.
Alternatively, to search for non-detections of known Solar System objects in prompt processing, join the ``SSObject`` and/or ``SSObservation`` table to the ``NearbySSO`` table using the ``ssObjectId``; non-detections will have NULL ``diaSourceIds``).

The ``NearbySSO`` table can be joined to the :doc:`SSObject <ss_object>` and :doc:`SSObservation <ss_observation>` tables on the ``ssObjectId`` or ``designation`` to obtain Rubin-derived object-level photometry and summary properties (``SSObject``) and Minor Planet Center (MPC)-submitted observations (``SSObservation``).

The PPDB contains all alerts from roughly September 2025 onwards as well as the accumulated Solar System tables. The ``NearbySSO`` catalog is rebuilt daily from the moving object observations Rubin submits to and are accepted by the MPC.

Processing
----------

The NearbySSO catalog is the result of :doc:`/processing/moving/index`.

Tutorials
---------

See the 200-level catalog :doc:`/tutorials/index` for a notebook on the Nearby SSO table.
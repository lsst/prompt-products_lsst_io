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

One row per nearest predicted known Solar System object for each ``DiaSource``, including the nearest objects predicted to land on a detector but match no ``DiaSource`` (with ``DiaSourceId`` = NULL). Unlike the ``SSObject`` and ``SSObservation`` tables that includes only observations (``SSObservation``) and objects (``SSObjects``) that have been submitted to and accepted by the MPC, the ``NearbySSO`` table only includes predictions for the nearest known Solar System objects to each ``DiaSource`` (or expected ``DiaSource`` given that a known Solar System object should be nearby). The ``NearbySSO`` table does NOT have detections of Solar System objects that have been passed through the MPC, only known objects that have been positionally matched to ``DiaSource`` detections or NULL ``DiaSource`` detections of known Solar System objects expected to be nearby.

The ``NearbySSO`` table includes the unique identifiers ``diaSourceId``, ``ssObjectId``, and ``designation``.
The ``ssObjectId`` and ``designation`` identifiers are those of moving objects in the ``SSObject`` table if the positionally matched ``DiaSource`` detection has been 'observed' and submitted to and accepted by the MPC.
A NULL ``ssObjectId`` or ``designation`` in ``NearbySSO`` means that the positionally matched ``DiaSource`` detection has not been processed through the MPC.
It also contains the ephemeris information (``ephRa``, ``ephDec``, and ``ephVmag``) with the predicted position's error ellipse (``ephRaErr``, ``ephDecErr``, ``ephRa_ephDec_Cov``).
``ephOffset`` provides the total observed versus predicted angular separation on the sky between the observed ``DiaSource`` and the predicted position of a known solar system object.

Because a single known moving object predicted position may fall within the 1-arcsecond association radius for multiple nearby ``DiaSources``, a ``diaDistanceRank`` field is included in the ``NearbySSO`` table with the rank of the ``diaSourceId`` -identified source in terms of its closeness to the predicted SSO position, among all ``DiaSources`` of the same visit within the object's matching radius of that prediction.
If the ``diaSourceId`` is the nearest ``DiaSource`` to this SSO prediction, ``diaDistanceRank`` = 1 would be set.
If it is the second nearest, it would be 2, etc, with ties broken by the lower ``diaSourceId``.
A rank above 1 means a closer ``DiaSource`` exists, which may be attributed to another object.
``diaDistanceRank`` is NULL for a prediction that matched no ``DiaSource``.

To explore how many objects have been positionally matched to ``DiaSource`` detections within a radius of some number of arcseconds, query ``NearbySSO``.
Alternatively, to search for non-detections of known Solar System objects in prompt processing, join the ``SSObject`` table to the ``NearbySSO`` table using the ``ssObjectId``; non-detections will have NULL ``diaSourceIds``).

The ``NearbySSO`` table can be joined to the :doc:`SSObject <ss_object>` and :doc:`SSObservation <ss_observation>` tables on the ``ssObjectId`` or ``designation`` to obtain Rubin-derived object-level photometry and summary properties (``SSObject``) and Minor Planet Center (MPC)-submitted observations (``SSObservation``). The ``NearbySSO`` table can also be joined to the :doc:`DiaSource <dia_source>` table on the ``diaSourceId``, should the nearest predicted known Solar Sytem object be positionally matched to a ``DiaSource`` detection.

The PPDB contains all alerts from roughly September 2025 onwards as well as the accumulated Solar System tables. The ``NearbySSO`` catalog is rebuilt daily using updated information from the MPC to include the predictions for the nearest known Solar System objects to each ``DiaSource`` detection. As additional detection/object linkages are made by the MPC and Solar System object ``designations`` and ``ssObjectId`` may change accordingly, the ``NearbySSO`` table is kept up-to-date daily with the most recent available information and provides the most up-to-date information for determining whether ``DiaSource`` detections have been positionally matched to known Solar System objects (but have not necessarily been 'observed' by Rubin and been submitted to and accepted by the MPC unless they appear in the ``SSObservation`` table).

Processing
----------

The NearbySSO catalog is the result of :doc:`/processing/moving/index`.

Tutorials
---------

See the 200-level catalog :doc:`/tutorials/index` for a notebook on the Nearby SSO table.
.. _catalogs-nearby-sso:

##########
Nearby SSO
##########

.. important::

   The Solar System tables are available via the TAP service as of Nov 30 2026.


Nearest predicted known Solar System object for each ``DiaSource``, and the predictions that matched no ``DiaSource`` (with a NULL ``diaSourceId``).

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

For each ``DiaSource``, ``NearbySSO`` gives the nearest known Solar System object predicted (from ``mpc_orbits``) within its matching radius: 5 arcseconds, or 15 arcseconds for comets and interstellar objects. Only predictions whose positional uncertainty is small enough for a meaningful match (a 1-sigma error ellipse semi-major axis of at most 10 arcseconds) are used. In addition, each such prediction that falls on, or within 30 arcseconds of, a detector of the visit but matched no ``DiaSource`` is included with a NULL ``diaSourceId``.

``NearbySSO`` is a statement about proximity, not identification: a row says that a known object was predicted near a ``DiaSource``, not that the ``DiaSource`` is that object. Unlike ``SSObservation`` and ``SSObject``, which contain only observations accepted by the MPC and the objects they belong to, ``NearbySSO`` covers every PPDB ``DiaSource``, whether or not it was submitted to the MPC.

The ``NearbySSO`` table includes the unique identifiers ``diaSourceId``, ``ssObjectId``, and ``designation``.
The ``designation`` (the unpacked primary provisional designation) of the predicted object is always present.
The ``ssObjectId`` is that object's identifier in ``SSObject``; it is NULL when the object has no ``SSObject`` row, i.e., when the MPC has accepted no Rubin observation of it yet.
It also contains the ephemeris information (``ephRa``, ``ephDec``, and ``ephVmag``) with the predicted position's error ellipse (``ephRaErr``, ``ephDecErr``, ``ephRa_ephDec_Cov``).
``ephOffset`` provides the total observed versus predicted angular separation on the sky between the observed ``DiaSource`` and the predicted position of a known solar system object.

Because a single known moving object's predicted position may fall within the matching radius of multiple nearby ``DiaSources``, a ``diaDistanceRank`` field is included in the ``NearbySSO`` table with the rank of the ``diaSourceId`` -identified source in terms of its closeness to the predicted SSO position, among all ``DiaSources`` of the same visit within the object's matching radius of that prediction.
If the ``diaSourceId`` is the nearest ``DiaSource`` to this SSO prediction, ``diaDistanceRank`` = 1 would be set.
If it is the second nearest, it would be 2, etc, with ties broken by the lower ``diaSourceId``.
A rank above 1 means a closer ``DiaSource`` exists, which may be attributed to another object.
``diaDistanceRank`` is NULL for a prediction that matched no ``DiaSource``.

To find which known objects were predicted near ``DiaSource`` detections, and how close, query ``NearbySSO``; compare ``ephOffset`` with the predicted position's uncertainty (``ephRaErr``, ``ephDecErr``) to judge how secure a match is.
To search for non-detections of known Solar System objects, select the rows with a NULL ``diaSourceId``.

The ``NearbySSO`` table can be joined to the :doc:`SSObject <ss_object>` and :doc:`SSObservation <ss_observation>` tables on the ``ssObjectId`` or ``designation`` to obtain Rubin-derived object-level photometry and summary properties (``SSObject``) and Minor Planet Center (MPC)-submitted observations (``SSObservation``). The ``NearbySSO`` table can also be joined to the :doc:`DiaSource <dia_source>` table on the ``diaSourceId``, for the rows that matched a ``DiaSource``.

The ``NearbySSO`` catalog is rebuilt daily from all PPDB ``DiaSources`` with the current MPC orbits and designations, so it reflects newly discovered objects, improved orbits, and designation changes. Predictions are evaluated at each row's ``midpointMjdTai``, corrected for the shutter's motion where a correction is available. A ``DiaSource`` is a confirmed observation of an object only if it appears in ``SSObservation``.

Processing
----------

The NearbySSO catalog is the result of :doc:`/processing/moving/index`.

Tutorials
---------

See the 200-level catalog :doc:`/tutorials/index` for a notebook on the Nearby SSO table.
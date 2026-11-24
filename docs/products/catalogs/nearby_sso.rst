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

As of Nov 30 2026, the DIA tables are accessible via the TAP service.
In early 2027, these tables will become accessible via the Butler (see the `timeline for early science <https://rubinobservatory.org/for-scientists/resources/early-science>`_).

TAP
---

|nearby_sso_doi|
* Table name: ``NearbySSO``


Description
===========

One row per nearest predicted known Solar System object for each ``DiaSource``, including the nearest objects predicted to land on a detector but match no ``DiaSource`` (with ``DiaSourceId`` = NULL).

To explore how many objects have been positionally matched to ``DiaSource`` detections within a radius of some number of arcseconds, query ``NearbySSO``. Alternatively, to search for non-detections of known Solar System objects in prompt processing, join the ``SSObject`` and/or ``SSObservation`` table to the ``NearbySSO`` table using the ``ssObjectId``; non-detections will have NULL ``diaSourceIds``).

The PPDB contains all alerts from roughly September 2025 onwards as well as the accumulated Solar System tables. The ``NearbySSO`` catalog is rebuilt daily from the moving object observations Rubin submits to and are accepted by the MPC.

Processing
----------

The NearbySSO catalog is the result of :doc:`/processing/moving/index`.

Tutorials
---------

See the 200-level catalog :doc:`/tutorials/index` for a notebook on the Nearby SSO table.
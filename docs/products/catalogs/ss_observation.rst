.. _catalogs-ss-observation:

##############
SS observation
##############

.. important::

   Prompt catalogs are not yet available.


MPC record of each Rubin-submitted observation for moving (Solar System) objects.

* Schema: `PPDB SS Observation table <https://sdm-schemas.lsst.io/ppdb.html#SSObservation>`_

Access
======

As of Nov 30 2026, the SS tables are accessible via the TAP service.
In early 2027, these tables will become accessible via the Butler (see the `timeline for early science <https://rubinobservatory.org/for-scientists/resources/early-science>`_).

TAP
---

|ss_observation_doi|
* Table name: ``SSObservation``


Description
===========

A "Solar System object" is a moving object for which one or more signal-to-noise (SNR) ratio >5 difference image detections (``DIASources``) or direct image detections (``Sources``) have been linked together either as an association to a previously known small body or as a new discovery.

``SSObservation`` includes one row per Minor Planet Center (MPC) record of each Rubin-submitted observation of a moving object, joined to the full measurement from which it was submitted (a difference image ``DiaSource`` or a direct image ``Source``) and its ephemeris from ``mpc_orbits``.
The ``SSObservation`` table contains the ``DiaSource``- or ``Source``-like columns for the difference image detection or direct image detection from which the Rubin observation was submitted to the MPC; there should thus never be a need to join the ``SSObservation`` and ``DiaSource`` tables.

The ``SSObservation`` table can be joined to the :doc:`SSObject <ss_object>`, :doc:`SSSource <ss_source>`, and :doc:`NearbySSO <nearby_sso>` tables on the ``ssObjectId`` or ``designation`` to obtain Rubin-derived object-level photometry and summary properties (``SSObject``), single-epoch measurements and observing geometry from alerts (``SSSource``), and nearest predicted known Solar System object per ``DiaSource`` (``NearbySSO``).

The PPDB contains all alerts from roughly September 2025 onwards as well as the accumulated Solar System tables. The ``SSObservation`` catalog is rebuilt daily from the moving object observations Rubin submits to and are accepted by the MPC.


Processing
----------

The SS observation catalog is the result of :doc:`/processing/moving/index`.

Tutorials
---------

See the 200-level catalog :doc:`/tutorials/index` for a notebook on the SS observation table.
.. _catalogs-ss-source:

#########
SS source
#########

.. important::

   Prompt catalogs are not yet available.


Instantaneous physical parameters for moving objects at the time of every observation.

Schema: `PPDB SSSource table <https://sdm-schemas.lsst.io/ppdb.html#SSSource>`_


Access
======

As of Nov 30 2026, the SS tables are accessible via the TAP service.
In early 2027, these tables will become accessible via the Butler (see the `timeline for early science <https://rubinobservatory.org/for-scientists/resources/early-science>`_).

TAP
---

* |ss_source_doi|
* Table name: ``SSSource``


Description
===========

A "SS source" is a signal-to-noise ratio > 5 detection in a difference image that has been associated with a known Solar System object.

The SS source table contains the 2-d (sky) coordinates and 3-d distances and velocities for every ``DiaSource`` alert associated with a known ``SSObject``.
Alerts for ``DiaSources`` with known Solar System objects predicted to be within the 1-arcsecond association radius of the ``DiaSource`` detection include the ``SSSource`` records for each Solar System object (SSO).
The ``SSSource`` table contains information derived from the known orbit of the associated Solar System object and the current observation of the ``DiaSource``.

Processing
----------

The SS source catalog is the result of :doc:`/processing/moving/index` and :doc:`/processing/association/index`.

Tutorials
---------

See the 200-level catalog :doc:`/tutorials/index` for a notebook on the SS source table.
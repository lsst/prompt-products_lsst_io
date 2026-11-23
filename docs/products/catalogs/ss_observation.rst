.. _catalogs-ss-observation:

############################
SS observation (coming soon)
############################

.. important::

   Prompt catalogs are not yet available.


MPC record of each Rubin-submitted observation for moving (Solar System) objects.


Access
======

**The SS observation catalog is not yet accessible.**

TAP
---

* Table name: ``SSObservation``
* Schema: `PPDB SS Observation table <https://sdm-schemas.lsst.io/ppdb.html#SSObservation>`_

Butler
------

* Dataset type: ('ss_observation', {}, ArrowAstropy)
* Format: Parquet


Description
===========

A "Solar System object" is a moving object for which groupings of difference image detections (``DiaSources``) or direct image detections (``Sources``) have been linked together.

One row per MPC record of each Rubin-submitted observation, joined to the full measurement from which it was submitted (a difference image ``DiaSource`` or a direct image ``Source``).

Processing
----------

The SS observation catalog is the result of :doc:`/processing/moving/index`.

Tutorials
---------

Coming soon.
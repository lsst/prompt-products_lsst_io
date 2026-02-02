.. _catalogs-ss-object:

#######################
SS object (coming soon)
#######################

.. important::

   Prompt catalogs are not yet available.


Derived parameters for moving (Solar System) objects.

|ss_object_doi|


Access
======

**The SS object catalog is not yet accessible.**

TAP
---

* Table name: ``SSObject``
* Schema: `APDB SS Object table <https://sdm-schemas.lsst.io/apdb.html#SSObject>`_

Butler
------

* Dataset type: ('ss_object', {}, ArrowAstropy)
* Format: Parquet


Description
===========

A "Solar System object" is a moving object for which groupings of difference image detections (``DIASources``) have been linked together.

The SS object table contains the unique ``SSObjectId`` identifier, number of observations, and the date of the discovery submission (if a new discovery) for each solar system object detected with signal-to-noise ratio >5.
It also contains derived parameters such as the phase angle, G12 slopes, and H magnitudes.

Processing
----------

The SS object catalog is the result of :doc:`/processing/moving/index`.

Tutorials
---------

Coming soon.
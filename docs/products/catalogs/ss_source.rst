.. _catalogs-ss-source:

#######################
SS source (coming soon)
#######################

.. important::

   Prompt catalogs are not yet available.


Instantaneous physical parameters for moving objects at the time of every observation.

|ss_source_doi|


Access
======

**The SS source catalog is not yet accessible.**


TAP
---

* Table name: ``SSSource``
* Schema: `APDB SS Source table <https://sdm-schemas.lsst.io/apdb.html#SSSource>`_

Butler
------

* Dataset type: ('ss_source', {}, ArrowAstropy)
* Format: Parquet


Description
===========

The SS source table contains the 2-d (sky) coordinates and 3-d distances and velocities for every ``SSObject`` at the time of every LSST detection of that ``SSObject`` (i.e., for every assicated ``DIASource``).

Processing
----------

The SS source catalog is the result of :doc:`/processing/moving/index`.

Tutorials
---------

Coming soon.
.. _catalogs-ss-source:

#########
SS source
#########

.. important::

   **Placeholder.** Prompt catalogs are not yet available.


Instantaneous physical parameters for moving objects at the time of every observation.

Schema:

Access
======

The SS source catalog is accessible via ...

**Recommended access service:**

TAP
---

* doi
* Table name:
* Columns:
* Rows:

Butler
------

* doi
* dataset type
* Format: Parquet
* Number of Butler datasets:


Description
===========

A "Solar System source" is a signal-to-noise ratio > 5 moving object detection in a visit image.

The SS source table contains the 2-d (sky) coordinates and 3-d distances and velocities for every ``SSObject`` at the time of every LSST observation of that ``SSObject``.

Processing
----------

The SS source catalog is the result of :doc:`/processing/moving/index`.

Tutorials
---------

TBD
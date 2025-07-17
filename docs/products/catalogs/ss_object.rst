.. _catalogs-ss-object:

#########
SS object
#########

.. important::

   **Placeholder.** Prompt catalogs are not yet available.


Derived parameters for moving (Solar System) objects.

Schema:

Access
======

The SS object catalog is accessible via ...

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

A "Solar System object" is a moving object for which groupings of difference image detections (``DIASources``) have been linked together.

The SS object table contains the unique ``SSObjectId`` identifier, number of observations, and the date of the discovery submission (if a new discovery) for each solar system object detected with signal-to-noise ratio >5.

The DP1 version of the SS object table is very minimal, with only 3 columns.
Future versions of this table will include, for example, the best fit absolute magnitude and slope (G and H) parameters per band and associated statistics.

Processing
----------

The SS object catalog is the result of :doc:`/processing/moving/index`.

Tutorials
---------

TBD
.. _catalogs-dia-forced-source:

#################
DIA forced source
#################

.. important::

   **Placeholder.** Prompt catalogs are not yet available.


Forced measurements in visit and difference images, at the coordinates of all DIA objects.

Schema:

Access
======

The DIA forced source catalog is accessible via ...

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

"Forced" photometry means a measurement made at a fixed coordinate in an image,
regardless of whether an above-threshold region was detected there in that particular image.

The DIA forced source table contains forced PSF flux photometry on both the visit (i.e., "direct" or "science")
and difference images at the coordinates of every object in the DIA object table.

Processing
----------

The DIA forced source catalog is the result of :doc:`/processing/dia/index`.

Tutorials
---------

TBD

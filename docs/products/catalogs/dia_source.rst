.. _catalogs-dia-source:

##########
DIA source
##########


.. important::

   **Placeholder.** Prompt catalogs are not yet available.


Measurements for sources detected in difference images.

Schema:

Access
======

The DIA source catalog is accessible via ...

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

A "DIA source" is a signal-to-noise ratio > 5 detection in a difference image.

The DIA source catalog contains measurements on a difference image
at the coordinates of every source detected in that difference image.
These measurements include PSF-fit and forced PSF fluxes, and aperture and
trailed-source fluxes.
Forced PSF fluxes on the corresponding visit (i.e., "direct" or "science") image
at the coordinates of the DIA source are also included.


Processing
----------

The DIA source catalog is the result of :doc:`/processing/dia/index`.

Tutorials
---------

TBD

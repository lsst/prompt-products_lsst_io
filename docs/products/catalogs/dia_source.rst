.. _catalogs-dia-source:

########################
DIA source (coming soon)
########################

.. important::

   Prompt catalogs are not yet available.


Measurements for sources detected in difference images.

|dia_source_doi|


Access
======

**The Prompt DIA source catalog is not yet accessible.**


TAP
---

* Table name: ``DiaSource``
* Schema: `APDB DIA Source table <https://sdm-schemas.lsst.io/apdb.html#DiaSource>`_

Butler
------

* Dataset type: ('dia_source', {**skymap**, **tract**}, ArrowAstropy)
* Format: Parquet


Description
===========

A "DIA source" is a signal-to-noise ratio > 5 detection in a difference image.

The DIA source catalog contains measurements on a difference image at the coordinates of every source detected in that difference image.
These measurements include PSF-fit and forced PSF fluxes, and aperture and trailed-source fluxes.
Forced PSF fluxes on the corresponding visit (i.e., "direct" or "science") image at the coordinates of the DIA source are also included.

All DIA sources are associated with either a :doc:`/products/catalogs/dia_object` or a :doc:`/products/catalogs/ss_object`.


Processing
----------

The DIA source catalog is the result of :doc:`/processing/dia/index`.

Tutorials
---------

Coming soon.

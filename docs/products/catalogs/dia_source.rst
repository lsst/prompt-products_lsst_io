.. _catalogs-dia-source:

##########
DIA source
##########

.. important::

   Prompt catalogs are not yet available.


Measurements for sources detected in difference images.

Schema: `PPDB DiaSource table <https://sdm-schemas.lsst.io/ppdb.html#DiaSource>`_


Access
======

As of Nov 30 2026, the DIA tables are accessible via the TAP service.
In early 2027, these tables will become accessible via the Butler (see the `timeline for early science <https://rubinobservatory.org/for-scientists/resources/early-science>`_).

TAP
---

* |DiaSource_doi|
* Table name: ``DiaSource``


Butler
------

* |dia_source_doi|
* Dataset type: ('dia_source', {**skymap**, **tract**}, ArrowAstropy)
* Format: Parquet


Description
===========

A "DIA source" is a signal-to-noise ratio > 5 detection in a difference image.

The DIA source catalog contains measurements on a difference image at the coordinates of every source detected in that difference image.
These measurements include PSF-fit and forced PSF fluxes, and aperture and trailed-source fluxes.
Forced PSF fluxes on the corresponding visit (i.e., "direct" or "science") and template images at the coordinates of the DIA source are also included.

All DIA sources are associated with either a :doc:`/products/catalogs/dia_object` or a :doc:`/products/catalogs/ss_object`.


Processing
----------

The DIA source catalog is the result of :doc:`/processing/dia/index`.

Tutorials
---------

See the 200-level catalog :doc:`/tutorials/index` for a notebook on the DIA source table.
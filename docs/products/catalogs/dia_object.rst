.. _catalogs-dia-object:

##########
DIA object
##########

.. important::

   Prompt catalogs are not yet available.


Derived properties for transient and variable objects detected in difference images.

Schema: `PPDB DiaObject table <https://sdm-schemas.lsst.io/ppdb.html#DiaObject>`_


Access
======

As of Nov 30 2026, the DIA tables are accessible via the TAP service.
In early 2027, these tables will become accessible via the Butler (see the `timeline for early science <https://rubinobservatory.org/for-scientists/resources/early-science>`_).

TAP
---

* |DiaObject_doi|
* Table name: ``DiaObject``

Butler
------

* |dia_object_doi|
* Dataset type: ('dia_object', {**skymap**, **tract**}, ArrowAstropy)
* Format: Parquet


Description
===========

A "DIA object" is an astrophysical transient or variable object at a static sky coordinate.
The DIA object table is created by associating DIA sources within a 1 arcsecond radius.
The DIA object table contains derived per-filter variability parameters (e.g., the minimum, mean, maximum and standard deviation of the difference-image fluxes).

**Note:** The `firstDiaSourceMjdTai` and `lastDiaSourceMjdTai` columns were added when alert production restarted at the end of October 2026, and they are unpopulated for any DIA object that was not redetected after that time.

Processing
----------

The DIA object catalog is the result of :doc:`/processing/dia/index`.


Tutorials
---------

See the 200-level catalog :doc:`/tutorials/index` for a notebook on the DIA object table.
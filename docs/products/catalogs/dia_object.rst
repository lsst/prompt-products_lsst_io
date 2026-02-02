.. _catalogs-dia-object:

########################
DIA object (coming soon)
########################

.. important::

   Prompt catalogs are not yet available.


Derived properties for transient and variable objects detected in difference images.

|dia_object_doi|


Access
======

**The Prompt DIA object catalog is not yet accessible.**


TAP
---

* Table name: ``DiaObject``
* Schema: `APDB DIA Object table <https://sdm-schemas.lsst.io/apdb.html#DiaObject>`_

Butler
------

* Dataset type: ('dia_object', {**skymap**, **tract**}, ArrowAstropy)
* Format: Parquet


Description
===========

A "DIA object" is an astrophysical transient or variable object at a static sky coordinate.
The DIA object table is created by associating DIA sources within a 1 arcsecond radius.
This table contains derived per-filter parameters (e.g., minimum/maximum flux, linear fit results).


Processing
----------

The DIA object catalog is the result of :doc:`/processing/dia/index`.


Tutorials
---------

Coming soon.
.. _catalogs-dia-forced-source:

###############################
DIA forced source (coming soon)
###############################

.. important::

   Prompt catalogs are not yet available.


Forced measurements in visit and difference images, at the coordinates of all DIA objects.

|dia_forced_source_doi|


Access
======

**The Prompt DIA forced source catalog is not yet accessible.**


TAP
---

* Table name: ``DiaForcedSource``
* Schema: `APDB DIA Source table <https://sdm-schemas.lsst.io/apdb.html#DiaForcedSource>`_


Butler
------

* Dataset type: ('dia_object_forced_source', {**skymap**, **tract**, **patch**}, ArrowAstropy)
* Format: Parquet


Description
===========

"Forced" photometry means a measurement made at a fixed coordinate in an image, regardless of whether an above-threshold region was detected there in that particular image.

The DIA forced source table contains forced PSF flux photometry on both the visit (i.e., "direct" or "science") and difference images at the coordinates of every object in the DIA object table.


Processing
----------

The DIA forced source catalog is the result of :doc:`/processing/dia/index`.


Tutorials
---------

Coming soon.

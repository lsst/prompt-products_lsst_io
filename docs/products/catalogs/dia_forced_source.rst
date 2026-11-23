.. _catalogs-dia-forced-source:

#################
DIA forced source
#################

.. important::

   Prompt catalogs are not yet available.


Forced measurements in visit and difference images, at the coordinates of all DIA objects.

Schema: `PPDB DiaForcedSource table <https://sdm-schemas.lsst.io/ppdb.html#DiaForcedSource>`_


Access
======

As of Nov 30 2026, the DIA tables are accessible via the TAP service.
In early 2027, these tables will become accessible via the Butler (see the `timeline for early science <https://rubinobservatory.org/for-scientists/resources/early-science>`_).


TAP
---

* |DiaForcedSource_doi|
* Table name: ``DiaForcedSource``


Butler
------

* |dia_object_forced_source_doi|
* Dataset type: ('dia_object_forced_source', {**skymap**, **tract**, **patch**}, ArrowAstropy)
* Format: Parquet


Description
===========

"Forced" photometry means a measurement made at a fixed coordinate in an image, regardless of whether an above-threshold region was detected there in that particular image.
The DIA forced source table contains forced PSF flux photometry on both the visit (i.e., "direct" or "science") and difference images at the coordinates some - but not all - DIA objects.

**Note:** Forced photometry on Prompt-processed images is only executed if a DIA object has at least 2 detections, and was not implemented prior to April 4 2026.


Processing
----------

The DIA forced source catalog is the result of :doc:`/processing/dia/index`.


Tutorials
---------

See the 200-level catalog :doc:`/tutorials/index` for a notebook on the DIA forced source table.
.. _catalogs-source:

###################
Direct-image source
###################

.. important::

   The direct-image source catalog is not yet available.


Measurements for sources detected in the direct images.

|source_doi|


Access
======

**The direct-image source catalog is not yet accessible.**

**The direct-image source catalog is proprietary**, as are the visit images from which they are derived.
These measurements are not included in alerts, and this catalog is not public like the rest of the PPDB; see :doc:`/access/index` for details.

Direct-image source catalogs will only be included as a Prompt Product up until the Data Release 1, which will include a source catalog.

Butler
------

* Dataset type: ('source', {**skymap**, **visit**, **detector**}, ArrowAstropy)
* Format: Parquet


Description
===========

A "source" is a signal-to-noise ratio > 5 detection in a direct image (i.e., a science image, not a difference image).

The source catalog contains PSF-fit measurements on a direct image at the coordinates of every source detected in that image.


Processing
----------

The source catalog is the result of :doc:`/processing/dia/index`.

Tutorials
---------

Coming soon.

.. _catalogs-source:

##################
Visit image source
##################

.. important::

   The visit image source catalog is not yet available.


Measurements for sources detected in the visit images.

|source_doi|


Access
======

**The visit image source catalog is not yet accessible.**

**The visit image source catalog is proprietary**, as are the visit images from which they are derived.
These measurements are not included in alerts and are not public like the rest of the PPDB; see :doc:`/access/index` for details.

Visit image source catalogs will only be included as a Prompt Product up until the Data Release 1, which will include a source catalog.

Butler
------

The visit image source catalog is only available via the Butler.

* Dataset type: ('source', {**skymap**, **visit**, **detector**}, ArrowAstropy)
* Format: Parquet


Description
===========

A "source" is a signal-to-noise ratio > 5 detection in a visit image (also called a "direct" or "science" image).

The source catalog contains PSF-fit measurements on a direct image at the coordinates of every source detected in that image.

Processing
----------

The source catalog is the result of running source detection on a :doc:`/products/images/visit_image`.

Tutorials
---------

Coming soon.

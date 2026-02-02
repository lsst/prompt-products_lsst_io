.. _images-visit-image:

#########################
Visit image (coming soon)
#########################

.. important::

   Prompt images are not yet available.


Individual processed and calibrated sky images.

|visit_image_doi|


Access
======

**The visit images are not yet accessible.**

**Embargo:** All promptly-processed visit images are subject to an 80-hour embargo period and will be released shortly after the embargo expires.

Butler
------

* Dataset type: ('visit_image', {band, **instrument**, day_obs, **detector**, physical_filter, **visit**}, ExposureF)
* Format: FITS

SIA and TAP
-----------

* Schema: `ObsCore table <https://sdm-schemas.lsst.io/ivoa_obscore.html>`_
* IVOA calibration level: 2
* Data Product Subtype: ``lsst.visit_image``


Description
===========

Raw images from the camera undergo processing that includes instrument signature removal (ISR), image calibration (photometric and astrometric), and PSF estimation.
The result is a fully calibrated visit image.

Each individual visit image contains data from one of the camera's detectors.

Processing
----------

The creation of visit images is described in :doc:`/processing/single-frame/index`.

Pixel data
----------

The visit images have three planes of pixel data.

* Image: sky pixel data in flux units of nJy.
* Variance: uncertainty (noise) in the flux in units of nJy^2.
* Mask: an integer bitmask of representative flag values.

Metadata
--------

The metadata for visit images retrieved from the Butler include information about the observation (e.g., pointing, weather), and the derived PSF, photometric calibration, and WCS.

Tutorials
---------

Coming soon.
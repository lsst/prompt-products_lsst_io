.. _images-visit-image:

###########
Visit image
###########

.. important::

   Prompt images are not yet available.


Individual processed and calibrated sky images.

Access
======

The visit images are not yet accessible.


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

Image: Sky pixel data in flux units of nJy.

Variance: Uncertainty (noise) in the flux in units of nJy^2.

Mask: An integer bitmask of representative flag values.

Metadata
--------

The metadata for visit images retrieved from the Butler include information about the observation (e.g., pointing, weather), and the derived PSF, photometric calibration, and WCS.

Tutorials
---------

TBD
.. _images-raw:

############
Raw exposure
############

.. important::

   Prompt images are not yet available.


Unprocessed exposure from camera readout.

Access
======

The raw exposures are not yet accessible.


Description
===========

Raw images from the camera are available exactly as they were read out of the camera.
The raw images should not be used for scientific analysis, and users should not attempt to rerun the instrument signature removal (ISR) processing.

Processing
----------

The processing of raw exposures is described in :doc:`/processing/single-frame/index` pipeline.

Pixel data
----------

The raw exposures have only an image plane, with no variance or mask planes like the visit images, because those planes are a result of image processing and calibration.

Image: sky pixel data in units of ADU (analog-digital units).

Metadata
--------

The metadata for raw exposures retrieved from the Butler include information about the observation (e.g., pointing, weather) and an initial WCS estimated from the telescope boresight.

Tutorials
---------

TBD
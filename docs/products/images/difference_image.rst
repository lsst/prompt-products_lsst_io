.. _images-difference-image:

################
Difference image
################

.. important::

   Prompt images are not yet available.


The result of subtracting a template coadd from a visit image.

Access
======

The difference images are not yet accessible.

Description
===========

Difference images are created by taking template coadd images, then resizing, warping, PSF-matching, and scaling them to a given visit image, and subtracting the result from the visit image.
Each individual difference image contains data from one of the camera's detectors.

Difference images contain sources of residual flux from the subtraction, both positive and negative.
Real astrophyisical transients, variables, and moving objects appear as sources with a flux that is equal to the difference between their flux in the template and visit images.

Processing
----------

The difference images are the result of :doc:`/processing/dia/index`.

Pixel data
----------

The difference images have three planes of pixel data.

Image: sky pixel data in flux units of nJy.

Variance: uncertainty (noise) in the flux in units of nJy^2.

Mask: an integer bitmask of representative flag values.

Metadata
--------

The metadata for difference images retrieved from the Butler include information about the observation (e.g., pointing, weather), and the derived PSF, photometric calibration, and WCS.

Tutorials
---------

TBD

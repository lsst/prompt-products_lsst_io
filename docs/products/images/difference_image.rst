.. _images-difference-image:

################
Difference image
################

.. important::

   **Placeholder.** Prompt images are not yet available.


The result of subtracting a template coadd from a visit image.

doi

Access
======

The difference images are accessible via ...

Butler
------

* dataset type
* Format:
* Number of Butler datasets:

SIA and TAP
-----------

Schema: `ObsCore table <https://sdm-schemas.lsst.io/ivoa_obscore.html>`_

IVOA calibration level: 3

Dataproduct subtype: ``lsst.difference_image``


Description
===========

Difference images are created by taking template coadd images, then
resizing, warping, PSF-matching, and scaling them to a given visit image,
and subtracting the result from the visit image.
Each individual difference image contains data from one of the camera's detectors.

Difference images contain sources of residual flux from the subtraction, both
positive and negative.
Real astrophyisical transients, variables, and moving objects appear as sources with
a flux that is equal to the difference between their flux in the template and visit images.

Processing
----------

The difference images are the result of :doc:`/processing/dia/index`.

Pixel data
----------

The difference images have three planes of pixel data.

Image: sky pixel data in flux units of nJy.

Variance: uncertainty (noise) in the flux in units of nJy^2.

Mask: an integer bitmask of representative flag values that indicate processing status or issues,
similar to the `SDSS bitmasks <https://www.sdss4.org/dr17/algorithms/bitmasks/>`_.

Metadata
--------

The metadata for difference images retrieved from the Butler include
information about the observation (e.g., pointing, weather),
and the derived PSF, photometric calibration, and WCS.

Tutorials
---------

TBD

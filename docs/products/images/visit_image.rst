.. _images-visit-image:

###########
Visit image
###########

.. important::

   **Placeholder.** Prompt images are not yet available.


Individual processed and calibrated sky images.

doi

Access
======

The visit images are accessible via ...

Butler
------

* dataset type
* Format:
* Number of Butler datasets:

SIA and TAP
-----------

Schema: `ObsCore table <https://sdm-schemas.lsst.io/ivoa_obscore.html>`_

IVOA calibration level: 2

Dataproduct subtype: ``lsst.visit_image``


Description
===========

Raw images from the camera undergo processing that includes
instrument signature removal (ISR),
image calibration (photometric and astrometric),
and PSF estimation.
The result is a fully calibrated visit image.

Each individual visit image contains data from one of the camera's detectors.

Processing
----------

The visit images are the result of :doc:`/processing/calibration/index`.

Pixel data
----------

The visit images have three planes of pixel data.

Image: Sky pixel data in flux units of nJy.

Variance: Uncertainty (noise) in the flux in units of nJy^2.

Mask: An integer bitmask of representative flag values that indicate processing status or issues,
similar to the `SDSS bitmasks <https://www.sdss4.org/dr17/algorithms/bitmasks/>`_.

Metadata
--------

The metadata for visit images retrieved from the Butler include
information about the observation (e.g., pointing, weather),
and the derived PSF, photometric calibration, and WCS.

Tutorials
---------

TBD
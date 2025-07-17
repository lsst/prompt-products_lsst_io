.. _images-template-coadd:

##############
Template coadd
##############

.. important::

   **Placeholder.** Prompt images are not yet available.


The combination of processed images with the best seeing, for a patch of sky and for each of the six LSST filters, to be used as a template for difference imaging.

doi

Access
======

The template coadd images are accessible via ...

Butler
------

* dataset type
* Format:
* Number of Butler datasets:

SIA and TAP
-----------

Schema: `ObsCore table <https://sdm-schemas.lsst.io/ivoa_obscore.html>`_

IVOA calibration level: 3

Dataproduct subtype: ``lsst.template_coadd``


Description
===========

The criteria for selecting input images is...

A mean stacking algorithm, weighted by inverse variance, combines selected exposures.

Each individual template coadd image covers a single patch of the sky:
a quadrilateral sub-region of the overall skymap that covers approximately 79 square arcminutes.
Patches slightly overlap at their edges.
Template coadd images are for a single filter.

Processing
----------

The creation of template coadd images is described in :doc:`/processing/templates/index`.

Pixel data
----------

The template coadd images have three planes of pixel data.

Image: sky pixel data in flux units of nJy.

Variance: uncertainty (noise) in the flux in units of nJy^2.

Mask: an integer bitmask of representative flag values that indicate processing status or issues,
similar to the `SDSS bitmasks <https://www.sdss4.org/dr17/algorithms/bitmasks/>`_.

Metadata
--------

The metadata for template coadd images retrieved from the Butler include a list of the input visit images,
and the derived PSF, photometric calibration, and WCS.

Tutorials
---------

TBD
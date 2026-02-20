.. _templates:

###################
Template generation
###################



Template coadded images are created by combining multiple calibrated images obtained in the same filter and of the same region of the sky.
This coaddition reduces noise, gaps, and artifacts.

Incremental Template Generation
===============================

In steady state operations, templates for alert production will be produced by the annual Data Releases.

During Early Operations prior to DR1, templates will be built incrementally when sufficient suitable images are available in a given area of the sky.
Once produced, templates will remain in place as long as possible to avoid repeated shifts of the difference light curves.

Template Availability
=====================

As of 2026 February, templates are deployed in the following discrete fields:

* COSMOS ``u``, ``g``, ``r``, ``i``, ``z``, ``y``
* ECDFS ``r``, ``i``, ``z``
* ELAISS1 ``g``, ``r``, ``i``, ``z``
* EDFS_A_B ``g``, ``r``, ``i``, ``z``
* XMM-LSS ``i``, ``z``
* M49 ``u``, ``g``, ``r``, ``i``


.. important::

   Tools for visualizing the current template holdings are under development.

Coadds by patch
===============

When producing coadded images, the processing pipeline refers to a skymap, which defines a grid of 2.8 square degree tracts covering the entire celestial sphere.
Each tract is further subdivided into 10 by 10 equally-sized patches, each covering approximately 79 square arcminutes.

To create a coadded image, the processing pipeline selects all suitable (i.e., surpassing certain quality thresholds) calibrated visit images covering a given patch in a given filter, warps them onto a single consistent pixel grid for that patch, then coadds them.

Each individual coadd image covers a single patch.

Patches at the edge of the survey area might not be entirely covered by the input visit images.
Unobserved sections of the deep coadd image have pixels with no data flagged as "NO_DATA" in the mask plane.

Input image selection
=====================

Image selection for coaddition must balance the desire for early templates (which necessarily have fewer images) with the need for good seeing, depth, artifact rejection, and spatial coverage to produce high-quality alerts.
Details of the image selection for incremental template building are still in development.


Image combination algorithm
===========================

A mean stacking algorithm, weighted by inverse variance, combines selected exposures.

To mitigate transient artifacts before coaddition, an artifact rejection procedure first identifies and masks features such as satellite trails, optical ghosts, and cosmic rays.

See "Coaddition Artifact Rejection and CompareWarp" (`dmtn-080.lsst.io <https://dmtn-080.lsst.io/>`_).
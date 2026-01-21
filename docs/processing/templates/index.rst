.. _templates:

###################
Template generation
###################

.. important::

   Prompt data processing is under construction.


Template coadded images are created by combining multiple calibrated images obtained in the same filter and of the same region of the sky.
This coaddition reduces noise, gaps, and artifacts.

Incremental Template Generation
===============================

Template Availability
=====================


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

For template coadds, good seeing (low PSF FWHM) is more important than depth.

*More details to be added here.*


Image combination algorithm
===========================

A mean stacking algorithm, weighted by inverse variance, combines selected exposures.

To mitigate transient artifacts before coaddition, an artifact rejection procedure first identifies and masks features such as satellite trails, optical ghosts, and cosmic rays.

See "Coaddition Artifact Rejection and CompareWarp" (`dmtn-080.lsst.io <https://dmtn-080.lsst.io/>`_).


Coadd background subtraction
============================

After image coaddition, a constant background residual is fit and subtracted.

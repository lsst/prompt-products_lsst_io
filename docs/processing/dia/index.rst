.. _dia:

###############################
Difference image analysis (DIA)
###############################

DIA uses the decorrelated `Alard and Lupton (1998) <https://ui.adsabs.harvard.edu/abs/1998ApJ...503..325A/abstract>`_ image differencing algorithm, as described in "Implementation of Image Difference Decorrelation" (`dmtn-021.lsst.io <https://dmtn-021.lsst.io/>`_) and in the LSST Science Pipelines documentation for the `Alard-Lupton implementation <https://pipelines.lsst.io/modules/lsst.ip.diffim/AL_implementation.html>`_.

Key terminology:

* ``DiaSource``: a detection, positive or negative, in a single difference image
* ``DiaObject``: the astrophysical transient or variable object at a static sky coordinate


Difference images
=================

Template coadd images are warped, scaled, and PSF-matched to a visit image, and then subtracted from the visit image to create a difference image.

In the context of DIA, the visit image is sometimes referred to as the "direct" or "science" image.


DIA source detection
====================

DIA source detection refers to the process of finding regions with above-threshold absolute flux in difference images, where the threshold is a signal-to-noise ratio of 5.

Difference images contain sources of residual flux from the subtraction, both positive and negative.
Real astrophysical transients, variables, and moving objects appear as sources with a flux that is equal to the difference between their flux in the template and visit images.
The ``scienceFlux`` column of the ``DiaSource`` catalog provides a forced PSF measurement on the direct image at the ``DiaSource`` position, which may be useful for scientists interested in variable objects with flux in the template.

Spurious sources are also present due to unavoidably imperfect template matching as well as other image artifacts.

Deblending is not currently run on difference image sources.

The results of DIA source detection are stored in the ``DiaSource`` catalog.

Dipoles
-------

Sources with footprints containing both positive and negative peaks are fit with a dipole centroid code.
Dipoles represent sources that are offset slightly (e.g., less than the PSF) between template and visit image.
Note that for very bright sources, dipoles occur even with minor displacements and don't indicate problems with astrometric calibration.

Trailed Sources
---------------

Trailed source parameters are estimated from the second moments of each ``DiaSource``.
Users interested in trailed sources are encouraged to filter on ``trailLength/trailLengthErr`` or other suitable parameters.

Pixel flags
-----------

If the source detection footprint contains one or more flagged pixels, e.g., for cosmic rays, detector edge, bad pixels, known artifacts, non-astrophysical trails, appropriate boolean flag columns are set.


DIA forced photometry
=====================

In general, "forced" photometry means a measurement made at a fixed coordinate in an image, regardless of whether an above-threshold region was detected in that particular image.

Forced PSF photometry measurements are made on all visit and difference images at the locations of DIA objects.
The results are stored in the ``DiaForcedSource`` catalog and included in alerts.
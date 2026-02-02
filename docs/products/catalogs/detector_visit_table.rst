.. _catalogs-detector-visit-table:

###############################################
Detector Visit Processing Summary (coming soon)
###############################################

.. important::

   Prompt catalogs are not yet available.


Observation metadata for individual detectors (e.g., date, time, band, PSF, zeropoint).

|detector_visit_doi|


Access
======

**The detector visit processing summary catalog is not yet accessible.**

TAP
---

* Table name: ``DetectorVisitProcessingSummary``
* Schema: `APDB DetectorVisitProcessingSummary table <https://sdm-schemas.lsst.io/apdb.html#DetectorVisitProcessingSummary>`_


Butler
------

* Dataset type: TBD
* Format: Parquet


Description
===========

A "detector" is one of the 189 CCDs (charge-coupled devices) that make up LSSTCam.
A "visit" is an observation in a single filter, obtained at a given time and sky coordinate.
A "detector visit" refers to an observation with a single detector as the reference for the observational metadata (e.g., airmass, seeing).

This table includes image characterization information measured from the image, including things such as the PSF size, sky background, and image zeropoint.

Tutorials
---------

Coming soon.
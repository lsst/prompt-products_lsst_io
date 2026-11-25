.. _solar-system-quality:

################################
Data quality and completeness
################################

Before you use the data for science, you'll want to know how good the measurements are, how to spot bad ones, and what the tables leave out.
This page covers each in turn.


Positions
=========

``ra`` and ``dec`` are the measured position of the source, and ``raErr``, ``decErr``, and ``ra_dec_Cov`` its uncertainty, as measured on the image.

To check a measurement against an object's orbit, compare it with the predicted position.
``SSObservation`` gives the prediction (``ephRa``, ``ephDec``, with uncertainties ``ephRaErr``, ``ephDecErr``, and ``ephRa_ephDec_Cov``) and the residuals directly:

- ``ephOffset`` is the total distance between the measured and predicted positions;
- ``ephOffsetRa`` and ``ephOffsetDec`` split it into components;
- ``ephOffsetAlongTrack`` and ``ephOffsetCrossTrack`` split it along and across the object's direction of motion.

A residual much larger than the combined uncertainties points to a problem with the measurement, or with the orbit.


Photometry
==========

``psfFlux`` is the object's flux, measured by fitting the point-spread function (PSF), in nanojanskys (nJy).
``psfMag`` is the same flux as an AB magnitude.
Fluxes and magnitudes are in Rubin's own filters, *ugrizy* (``band``), not in Johnson V.
The predicted magnitude ``ephVmag`` is a V magnitude, from the MPC's absolute magnitude.

``psfFlux`` is measured on the image the detection was made on (``measuredOn``).
On a difference image the object's flux remains because the template doesn't contain the moving object.
If it does, for instance because the object was in the field when the template's images were taken, the flux and position can be biased.

``apFlux`` is the flux in a 12-pixel-radius aperture, available for nearly every row.
Rows measured on visit images (``measuredOn = 'science'``) also have 3-, 6-, and 25-pixel apertures (``ap03Flux``, ``ap06Flux``, ``ap25Flux``), useful for looking for comae.
Aperture fluxes are not corrected for the light outside the aperture.


Flags
=====

Each measurement carries the flags of the pipeline that made it.
The most useful ones to check are:

- ``pixelFlags_saturated``, ``pixelFlags_cr`` (cosmic ray), ``pixelFlags_edge``, ``pixelFlags_bad``, ``pixelFlags_suspect``, and ``pixelFlags_streak``, which mark problem pixels in or near the source (the ``...Center`` versions mark the central pixels);
- ``centroid_flag``, set when the position measurement failed;
- ``psfFlux_flag``, set when the PSF flux measurement failed;
- ``trail_flag``, set when the fit of a trailed-source model failed (filled only for recent alert processing);
- ``glint_trail``, set when the source is part of a satellite glint trail.

``extendedness`` (0 for point-like, 1 for extended) and, for difference-image rows, ``reliability`` (the probability, from a machine-learning model, that the detection is real) are also useful.

These are boolean columns, and the PPDB's TAP service can't currently filter on boolean columns.
Retrieve the flags with your query and apply the cuts afterwards, as in :doc:`recipes`.


Phase curves and colors
=======================

``SSObject`` gives, for each band, an absolute magnitude H and slope G12, fitted to the object's PSF photometry with the H-G12 phase function.
How the fit is done, and when the slope falls back to a fixed value, is described in :doc:`tables`.

Two cautions apply to using these values for colors.
First, the bands are fitted separately, each over its own range of phase angles, so differences between bands (for example ``g_H - r_H``) can be thrown off when one band's coverage is poor.
Second, rotation is not modeled: an object's brightness changes as it rotates, and that variation goes into the fit's scatter.
Population-wide, the colors behave as expected; for individual objects, look at ``{band}_nObsUsed``, the phase-angle ranges, and ``{band}_slope_fit_failed`` before trusting a color.

Rubin does not provide rotation periods, light-curve amplitudes, taxonomic classes, sizes, or albedos.
The individual observations in ``SSObservation`` are there for you to derive them.


What the tables contain, and what they don't
============================================

``SSObservation`` contains a detection only when **all** of the following are true:

- Rubin detected it, on a difference image or, where no template existed, on a visit image;
- it was associated with a known object or linked into a new one, and submitted to the MPC;
- the MPC accepted and published it.

So the tables do **not** contain:

- detections the MPC has not yet accepted and published;
- single-night detections that can't yet be linked to an object (singletons), which are currently not being submitted to the MPC, at the MPC's request;
- objects Rubin saw but has not linked;
- fields observed before a template existed for them, except where Rubin measured moving objects on the visit images.

These selection effects depend on the field, the time, and the state of Rubin's processing, and they are not characterized in the PPDB.
The PPDB is therefore well suited to studying individual objects and to looking for interesting ones, but not to measuring the sizes or properties of populations.
For population studies, use a data release, whose selection is characterized.

**Non-detections.**
``NearbySSO`` rows with a NULL ``diaSourceId`` mark visits where a known object was predicted to fall on, or near, a detector, but no ``DiaSource`` matched it.
That can happen because the object was too faint, because it fell in a gap between detectors or on masked pixels, because no template existed, or because its orbit is uncertain.
The PPDB does not record each visit's depth, so these rows are not, on their own, upper limits on brightness.


Where to go next
================

:doc:`tables` describes each table and its most useful columns.

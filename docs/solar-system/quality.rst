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
On a difference image, the moving object's flux survives the subtraction because the template, built from other nights, normally doesn't contain the object.
If the template happens to contain it, the difference-image flux and position can be biased.

``apFlux`` is the flux in a 12-pixel-radius aperture, available for nearly every row.
Rows measured on visit images (``measuredOn = 'science'``) also have 3-, 6-, and 25-pixel apertures (``ap03Flux``, ``ap06Flux``, ``ap25Flux``), useful for looking for comae.
Aperture fluxes are not corrected for the light outside the aperture.

For fast-moving objects, difference-image rows also have the results of a trailed-source fit: ``trailFlux``, ``trailLength`` (in arcseconds), and ``trailAngle``.


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

Each ``{band}_H`` is an absolute magnitude in that Rubin band, on the AB system.
It is not directly comparable with ``mpc_orbits.h``, the MPC's V-band absolute magnitude in the H-G system.

Three cautions apply to using these values for colors:

- The bands are fitted separately, each over its own range of phase angles, so a difference such as ``g_H - r_H`` can be thrown off when one band's coverage is poor.
- If one band's slope was fitted and the other's fell back to G12 = 0.5, the two H values rest on different phase functions. Compare bands that were treated alike.
- Rotation is not modeled: an object's brightness changes as it rotates, and that variation goes into the fit's scatter.

Before trusting the color of an individual object, look at ``{band}_nObsUsed``, the phase-angle ranges, and ``{band}_slope_fit_failed``.

Rubin does not provide rotation periods, light-curve amplitudes, taxonomic classes, sizes, or albedos.
The individual observations in ``SSObservation`` are there for you to derive them.
The tables also don't currently record which objects Rubin discovered.


What the tables contain, and what they don't
============================================

As :doc:`concepts` explains, ``SSObservation`` contains a detection only if Rubin detected it, associated or linked it to an object and submitted it, and the MPC accepted and published it.
So the tables do not contain detections the MPC hasn't yet published, singletons, or objects Rubin saw but hasn't linked.
Fields observed before a template existed for them are covered only where Rubin measured moving objects on the visit images.

These selection effects depend on the field, the time, and the state of Rubin's processing, and they are not characterized in the PPDB.
The PPDB is therefore well suited to studying individual objects and to looking for interesting ones, but not to measuring the sizes or properties of populations.
For population studies, use a data release, whose selection is characterized.

**Non-detections.**
``NearbySSO`` rows with a NULL ``diaSourceId`` mark visits where a known object was predicted to fall on a detector, or within 30 arcseconds of one, but no ``DiaSource`` matched it.
So a row does not guarantee that the object was on the detector: it may have fallen in a gap between detectors (``detector`` is then the nearest one).
Even when it was on the detector, it may have gone undetected because it was too faint, because it fell on masked pixels, because no template existed, or because its orbit is uncertain.
The PPDB does not record each visit's depth, so these rows are not, on their own, upper limits on brightness.


Where to go next
================

:doc:`recipes` puts all this to work.

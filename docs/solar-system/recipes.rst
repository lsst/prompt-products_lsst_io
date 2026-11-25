.. _solar-system-recipes:

#######
Recipes
#######

Copy-and-paste ADQL queries for common tasks.
Each one runs as written in the PPDB; change the designation, band, or cuts to suit your science.
The last section shows how to run them from Python and plot the result.


An object's observations
========================

By designation, with everything you need for a phase curve: the phase angle, the distances from the Sun and Earth (in au), and the magnitude reduced to unit distances.

.. code-block:: sql

   SELECT midpointMjdTai, band, phaseAngle, helioRange, topoRange,
          psfMag, psfMagErr,
          psfMag - 5 * LOG10(helioRange * topoRange) AS reducedMag
   FROM ppdb.SSObservation
   WHERE designation = '1993 FG21'
   ORDER BY band, phaseAngle

By number: translate the number to the primary designation with ``numbered_identifications``.
This works for numbered comets too, for example ``permid = '29P'``.

.. code-block:: sql

   SELECT o.midpointMjdTai, o.band, o.ra, o.dec, o.psfMag, o.psfMagErr
   FROM ppdb.SSObservation AS o
   JOIN ppdb.numbered_identifications AS n
     ON o.designation = n.unpacked_primary_provisional_designation
   WHERE n.permid = '100099'
   ORDER BY o.midpointMjdTai

From another designation: join ``current_identifications`` the same way, on ``unpacked_primary_provisional_designation``, and select on ``unpacked_secondary_provisional_designation``.


A light curve
=============

One band, in time order, with the time corrected for light travel time.
The correction subtracts the light time, ``topoRange`` multiplied by 0.0057755183 days per au.

.. code-block:: sql

   SELECT midpointMjdTai,
          midpointMjdTai - 0.0057755183 * topoRange AS lightTimeCorrectedMjdTai,
          psfMag, psfMagErr, topoRange, helioRange, phaseAngle
   FROM ppdb.SSObservation
   WHERE designation = '1993 FG21' AND band = 'r'
   ORDER BY midpointMjdTai


An object's summary and orbit
=============================

.. code-block:: sql

   SELECT s.designation, s.nObs, s.arc,
          s.r_H, s.r_HErr, s.r_G12, s.r_G12Err, s.r_nObsUsed,
          m.a, m.e, m.i, m.q, m.epoch_mjd
   FROM ppdb.SSObject AS s
   JOIN ppdb.mpc_orbits AS m ON s.designation = m.designation
   WHERE s.designation = '1993 FG21'


Selecting a population by orbit
===============================

Join ``SSObject`` to ``mpc_orbits`` and cut on the orbital elements.
Trans-Neptunian objects (semi-major axis beyond Neptune's), with their r-band absolute magnitudes:

.. code-block:: sql

   SELECT s.designation, m.a, m.e, m.i, m.q, s.r_H, s.r_HErr, s.nObs
   FROM ppdb.SSObject AS s
   JOIN ppdb.mpc_orbits AS m ON s.designation = m.designation
   WHERE m.a > 30.07 AND s.r_H IS NOT NULL
   ORDER BY s.r_H

Other populations need only a different cut, for example:

- near-Earth objects: ``m.q < 1.3``;
- Centaurs: ``m.a > 5.5 AND m.a < 30.07 AND m.q > 5.2``.

For objects on Jupiter-family comet orbits, use the Tisserand parameter in ``SSObject``.
This query returns all their observations:

.. code-block:: sql

   SELECT o.designation, o.midpointMjdTai, o.band, o.ra, o.dec, o.psfMag, o.psfMagErr,
          s.tisserand_J
   FROM ppdb.SSObservation AS o
   JOIN ppdb.SSObject AS s ON o.ssObjectId = s.ssObjectId
   WHERE s.tisserand_J BETWEEN 2 AND 3
   ORDER BY o.designation, o.midpointMjdTai

The cut 2 < T\ :sub:`J` < 3 also selects Jupiter Trojans, Hildas, and some outer main-belt asteroids; add cuts on ``mpc_orbits.a``, ``e``, or ``q`` to exclude them.


Phase-curve fits
================

r-band fits where the slope was fitted, from at least 20 observations.
(When the slope can't be fitted, G12 falls back to 0.5 and ``r_G12Err`` is NULL; the query uses that to select real fits, because the PPDB's TAP service can't filter on the boolean ``r_slope_fit_failed``.)

.. code-block:: sql

   SELECT designation, r_H, r_HErr, r_G12, r_G12Err, r_nObsUsed,
          r_phaseAngleMin, r_phaseAngleMax
   FROM ppdb.SSObject
   WHERE r_G12Err IS NOT NULL AND r_nObsUsed >= 20


Colors
======

Colors from the per-band absolute magnitudes, for objects with at least five observations and a fitted slope in each band:

.. code-block:: sql

   SELECT designation, g_H - r_H AS g_r, r_H - i_H AS r_i
   FROM ppdb.SSObject
   WHERE g_nObsUsed >= 5 AND r_nObsUsed >= 5 AND i_nObsUsed >= 5
     AND g_G12Err IS NOT NULL AND r_G12Err IS NOT NULL AND i_G12Err IS NOT NULL

Removing the ``G12Err`` conditions gives many more objects, but mixes fits whose slopes were fitted with fits that fell back to G12 = 0.5.
Read the cautions in :doc:`quality` before using colors of individual objects.


Comets
======

Observations of objects with cometary designations, with aperture magnitude, extendedness, and the directions in which a tail would point:

.. code-block:: sql

   SELECT o.designation, o.midpointMjdTai, o.band, o.psfMag, o.apMag,
          o.extendedness, o.helioRange, o.ephAntiSunPA, o.ephAntiMotionPA
   FROM ppdb.SSObservation AS o
   WHERE o.designation LIKE 'C/%' OR o.designation LIKE 'P/%'
      OR o.designation LIKE 'D/%' OR o.designation LIKE 'I/%'
   ORDER BY o.designation, o.midpointMjdTai

Active asteroids and other objects with asteroidal designations are not selected by this query; select them by designation or by orbit.


Everything Rubin saw in a field
===============================

Solar System observations within one degree of a position, in a ten-day window:

.. code-block:: sql

   SELECT designation, midpointMjdTai, band, ra, dec, psfMag
   FROM ppdb.SSObservation
   WHERE CONTAINS(POINT('ICRS', ra, dec), CIRCLE('ICRS', 148.7, 1.3, 1.0)) = 1
     AND midpointMjdTai BETWEEN 61050 AND 61060


What known object could this detection be?
==========================================

The detections of one visit whose nearest predicted object lies within three times the prediction's uncertainty:

.. code-block:: sql

   SELECT n.diaSourceId, n.designation, n.ephOffset,
          n.ephRaErr * 3600 AS ephRaErr_arcsec, n.ephDecErr * 3600 AS ephDecErr_arcsec
   FROM ppdb.NearbySSO AS n
   WHERE n.visit = 2026011200081
     AND n.diaSourceId IS NOT NULL
     AND n.diaDistanceRank = 1
     -- ephOffset is in arcseconds; ephRaErr and ephDecErr are in degrees
     AND n.ephOffset < 3 * 3600 * SQRT(n.ephRaErr * n.ephRaErr + n.ephDecErr * n.ephDecErr)

To get the detections' measurements, join ``ppdb.DiaSource`` on ``diaSourceId``.


Where was an object predicted but not detected?
===============================================

The visits where a known object was predicted to be on or near a detector, but no detection matched it.
See :doc:`quality` for why these are not upper limits on their own.

.. code-block:: sql

   SELECT visit, detector, ephRa, ephDec, ephVmag
   FROM ppdb.NearbySSO
   WHERE designation = '2006 SH230' AND diaSourceId IS NULL


From Python
===========

In a notebook on the RSP, get the PPDB TAP service and run any of the queries above.
This example gets the observations of an object, removes measurements with problem flags, and plots the phase curve in each band.

.. code-block:: python

   import matplotlib.pyplot as plt
   from lsst.rsp import get_tap_service

   service = get_tap_service("ppdbtap")

   query = """
   SELECT band, phaseAngle, psfMag, psfMagErr,
          psfMag - 5 * LOG10(helioRange * topoRange) AS reducedMag,
          psfFlux_flag, centroid_flag, pixelFlags_saturated, pixelFlags_cr
   FROM ppdb.SSObservation
   WHERE designation = '1993 FG21'
   """
   obs = service.search(query).to_table().to_pandas()

   # The TAP service can't filter on boolean columns, so cut on the flags here.
   flags = [
       "psfFlux_flag",
       "centroid_flag",
       "pixelFlags_saturated",
       "pixelFlags_cr",
   ]
   good = obs[~obs[flags].fillna(False).any(axis=1)]

   for band, rows in good.groupby("band"):
       plt.errorbar(
           rows.phaseAngle, rows.reducedMag, rows.psfMagErr, fmt="o", label=band
       )
   plt.gca().invert_yaxis()
   plt.xlabel("Phase angle (deg)")
   plt.ylabel("Reduced magnitude")
   plt.legend()

To look up your own list of objects, upload it as a table and join to it:

.. code-block:: python

   from astropy.table import Table

   mine = Table({"des": ["1930 BM", "1997 CU26"]})
   query = """
   SELECT u.des, COUNT(*) AS nobs
   FROM TAP_UPLOAD.mine AS u
   JOIN ppdb.SSObservation AS o ON o.designation = u.des
   GROUP BY u.des
   """
   counts = service.run_sync(query, uploads={"mine": mine}).to_table()

For large queries, use ``service.submit_job(query)`` and run them one at a time (see :doc:`caveats`).
To fit phase curves yourself, packages such as `sbpy <https://sbpy.org/>`_ implement the standard phase functions.


Where to go next
================

:doc:`caveats` lists the things that most often trip people up.

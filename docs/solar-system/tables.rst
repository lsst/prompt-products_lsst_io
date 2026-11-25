.. _solar-system-tables:

##########
The tables
##########

This page is a tour of the Solar System tables: what each row is, the columns you'll use first, and how the tables join.
Each table's page under :doc:`/products/catalogs/index` lists the rest of its columns, and the `PPDB schema browser <https://sdm-schemas.lsst.io/ppdb.html>`_ has every column's definition.


SSObservation: Rubin's observations
===================================

One row per Rubin observation that the MPC has accepted.
Each row combines the MPC's record of the observation, the full Rubin measurement, and the object's predicted position and observing geometry.

Columns to start with:

- ``designation`` and ``ssObjectId``: the object.
- ``midpointMjdTai``: the time (TAI), corrected for the shutter's motion.
- ``band``, ``ra``, ``dec``, ``psfMag``, ``psfMagErr``: the measurement.
- ``phaseAngle``, ``helioRange``, ``topoRange``, ``elongation``: the observing geometry, from the orbit (angles in degrees, distances in au).
- ``ephRa``, ``ephDec``, ``ephOffset``: the predicted position and the residual from it.
- ``ephAntiSunPA`` and ``ephAntiMotionPA``: the position angles of the anti-Sun and anti-motion directions, where a comet's ion tail and dust trail point.
- ``helio_x``, ``helio_y``, ``helio_z`` and ``topo_x``, ``topo_y``, ``topo_z`` (with their velocities): the object's heliocentric and topocentric state vectors.
- ``obsid``, ``trksub``, ``status``: the MPC's record of the observation.
- ``processing`` and ``measuredOn``: where the measurement comes from (see :doc:`concepts`).

Full reference: :doc:`/products/catalogs/ss_observation`.


SSObject: one row per object
============================

One row per object with at least one Rubin observation accepted by the MPC.
Everything in it is computed from the object's ``SSObservation`` rows and its MPC orbit.

Columns to start with:

- ``designation`` and ``ssObjectId``.
- ``nObs``, ``arc``, ``firstObservationMjdTai``: how many Rubin observations there are, over what span, and since when.
- ``{band}_H``, ``{band}_G12``, and their uncertainties, for each band *ugrizy*: the phase-curve fits (below). H is an AB magnitude in the Rubin band.
- ``{band}_nObs``, ``{band}_nObsUsed``, ``{band}_phaseAngleMin``, ``{band}_phaseAngleMax``: what each band's fit is based on (phase angles in degrees).
- ``MOIDEarth``: the minimum orbit intersection distance with Earth's orbit; ``MOIDEarthDeltaV``, ``MOIDEarthEclipticLongitude``, ``MOIDEarthTrueAnomaly``, and ``MOIDEarthTrueAnomalyObject`` describe where on the two orbits it occurs.
- ``tisserand_J``: the Tisserand parameter with respect to Jupiter.
- ``extendednessMin``, ``extendednessMedian``, ``extendednessMax``: how extended the object's detections look.

How the phase curves are fitted
-------------------------------

In each band, H and G12 are fitted together to the object's PSF magnitudes, with the H-G12 phase function and with G12 limited to the range 0 to 1.
A 0.05 mag floor is added in quadrature to each magnitude's uncertainty, and outliers are rejected with a robust fit before the final fit.

When the data can't constrain the slope, ``{band}_slope_fit_failed`` is true: G12 is fixed at 0.5 and only H is fitted, and ``{band}_G12Err`` and the H-G12 covariance are NULL.
That happens when:

- fewer than three observations are usable;
- the observations span less than 5 degrees of phase angle;
- the fitted G12 lands at 0 or 1, or at 0.2, where the G12 function changes form; or
- the fit is singular.

Early in the survey most objects have narrow phase coverage, so many fits fall back to G12 = 0.5.

The MOID and Tisserand parameter are computed from the MPC orbit.
On their own, they should not be used to assess impact hazard or predict close approaches.

Full reference: :doc:`/products/catalogs/ss_object`.


NearbySSO: what could this detection be?
========================================

For each detection in a difference image (``DiaSource``), the nearest known object predicted to be within 5 arcseconds of it, or 15 arcseconds for comets and interstellar objects.
Only objects whose predicted position is known to better than 10 arcseconds are considered.
It also lists, with a NULL ``diaSourceId``, the known objects predicted to fall on or near a detector that matched no detection.

Columns to start with:

- ``diaSourceId``, ``visit``, ``detector``: the detection, or for unmatched predictions, where the object was predicted.
- ``designation`` (always present) and ``ssObjectId`` (NULL if the object has no ``SSObject`` row yet).
- ``ephRa``, ``ephDec``, ``ephVmag``, and the position's uncertainty (``ephRaErr``, ``ephDecErr``, ``ephRa_ephDec_Cov``).
- ``ephOffset``: the distance between the detection and the prediction. Compare it with the uncertainty to judge the match.
- ``diaDistanceRank``: 1 if this is the nearest detection to the prediction; higher if a closer detection exists.

Full reference: :doc:`/products/catalogs/nearby_sso`.


The MPC tables
==============

Daily copies of three tables from the `MPC database <https://data.minorplanetcenter.net/mpcops/documentation/replicated-tables-schema/>`_, unchanged and with the MPC's definitions:

- ``mpc_orbits``: the MPC's orbits for all known objects (``a``, ``e``, ``i``, ``q``, ``node``, ``argperi``, ``epoch_mjd`` in TT, ``h``, ``g``, uncertainties, and fit information), with a ``designation`` column added for joining.
- ``numbered_identifications``: permanent numbers (``permid``) and the primary designations they belong to.
- ``current_identifications``: secondary designations and the primary designations they belong to.

The orbits are the MPC's, computed from the observations of all observatories, not only Rubin's.
The predicted positions in ``SSObservation`` and ``NearbySSO`` are computed from them.

Full reference: :doc:`/products/catalogs/mpc_orbits`, :doc:`/products/catalogs/numbered_identifications`, :doc:`/products/catalogs/current_identifications`.


Asteroids, comets, NEOs, and TNOs
=================================

All kinds of small bodies are in the same tables; there are no separate tables for comets or near-Earth objects.

- **Comets** appear like any other object, identified by designations such as ``C/2023 H1`` or ``P/1927 V1`` (29P).
  They are matched within 15 arcseconds in ``NearbySSO``.
  ``SSObject`` fits them with the H-G12 phase function too, which is not meaningful for an active comet.
- **NEOs** appear once the MPC has designated them and accepted Rubin's observations.
  Candidates on the MPC's NEO Confirmation Page are not in the tables.
- **TNOs and other distant objects** appear once the MPC has accepted the discovery observations.

To select a population, join to ``mpc_orbits`` and cut on the orbital elements; :doc:`recipes` has examples.


How the tables join
===================

- ``designation`` joins ``SSObservation``, ``SSObject``, ``NearbySSO``, and ``mpc_orbits``, and matches ``unpacked_primary_provisional_designation`` in the identification tables.
- ``ssObjectId`` joins ``SSObservation``, ``SSObject``, and ``NearbySSO``.
- ``diaSourceId`` joins ``NearbySSO`` to the PPDB's ``DiaSource`` table.


Which table do I need?
======================

.. list-table::
   :header-rows: 1
   :widths: 55 45

   * - To find...
     - Use
   * - Rubin's observations of an object
     - ``SSObservation``
   * - An object's absolute magnitudes, phase-curve slopes, MOID, or Tisserand parameter
     - ``SSObject``
   * - An object's orbit
     - ``mpc_orbits``
   * - Known objects that could explain a detection
     - ``NearbySSO``
   * - Where a known object should have been seen but wasn't
     - ``NearbySSO`` (rows with a NULL ``diaSourceId``)
   * - The primary designation for a number or another designation
     - ``numbered_identifications`` or ``current_identifications``


Where to go next
================

:doc:`quality` covers the uncertainties and flags, and what the tables do and don't contain.

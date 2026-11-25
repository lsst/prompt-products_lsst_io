.. _moving:

#############################
Solar System Processing (SSP)
#############################

Solar System Processing includes both the association of known Solar System Objects (SSOs) during nighttime observations (see the :doc:`/processing/association/index` page) as well as the discovery of new SSOs by linking unassociated ``DiaSources`` during daytime processing.
Rubin reports its SSO observations and discoveries to the Minor Planet Center (MPC).
The MPC identifies the observations, assigns designations, and computes the orbits of all known objects.
Rubin computes per-object properties, such as per-band absolute magnitudes, from its observations accepted by the MPC.
Single-night detections not yet linked to an object (singletons) are currently not being submitted to the MPC, at the MPC's request.


General overview of SSP
=======================

.. toctree::
    :maxdepth: 1
    :titlesonly:
    :glob:

    ss_prompt
    ss_linking

Solar System processing consists of two key components: the association of observations (sources) with known solar system objects, and the discovery of previously unknown objects by linking sets of tracklets, where a tracklet is defined as two or more observations taken in close succession in a single night.

To generate expected positions for known objects, ephemerides are computed for all objects found in the Minor Planet Center orbit catalog using the ASSIST integrator (`Holman et al. 2023 <https://github.com/matthewholman/assist>`_) in the ``Sorcha`` survey simulation toolkit (`Merritt et al. 2025 <https://github.com/dirac-institute/sorcha>`_).
To enable fast lookup of objects potentially present in an observed visit, ``Sorcha`` uses the ``mpsky`` package (M. Juric 2025).
In each difference image resulting from :doc:`/processing/dia/index` processing, the closest ``DIASource`` within 1 arcsecond of a known solar system object’s predicted position is associated to that object (for details, see the :doc:`/processing/association/index` page).

Solar system object discovery uses the ``heliolinx`` package of asteroid identification and linking tools (`A. Heinze et al. 2023 <https://ui.adsabs.harvard.edu/abs/2023DPS....5540503H/abstract>`_).
The suite consists of the following tasks:

- Tracklet creation with ``make_tracklets``
- Multi-night tracklet linking with ``heliolinc``
- Linkage post processing (orbit fitting, outlier rejection, and de-duplication) with ``link_purify``

Tracklet creation with ``make_tracklets`` uses an upper limit angular velocity of 1.5 deg/day, faster than any main belt asteroid and in the range of many NEO discoveries.
To avoid excessive false tracklets from fields observed many times per night, the minimum tracklet length is set to three and the minimum on-sky motion for a valid tracklet is set to five arcseconds.

The heart of the discovery pipeline is the ``heliolinc`` task, which connects (“links”) tracklets belonging to the same object over a series of nights.
It employs the HelioLinC3D algorithm (`S. Eggl et al. 2020 <https://ui.adsabs.harvard.edu/abs/2020DPS....5221101E/abstract>`_; `A. Heinze et al. 2022 <https://ui.adsabs.harvard.edu/abs/2022DPS....5450404H/abstract>`_), a refinement of the original HelioLinC algorithm of `M. J. Holman et al. (2018) <https://ui.adsabs.harvard.edu/abs/2018AJ....156..135H/abstract>`_.
The ``heliolinc`` run tests each tracklet with 324 different hypotheses spanning heliocentric distances from 1.5 to 9.8 au and radial velocities spanning the full range of possible bound orbits (eccentricity 0.0 to nearly 1.0).
This range of distances encompasses all main belt asteroids and Jupiter Trojans, as well as many comets and Mars-crossers and some NEOs.
Smaller heliocentric distances are not attempted, because nearby objects move rapidly across the sky and hence were not likely to remain long enough in the field to be discovered.
A clustering radius is chosen corresponding to :math:`1.33 \times 10^{-3}` au at 1 au from Earth.
Linkages produced by ``heliolinc`` are post-processed with ``link_purify`` into a final non-overlapping set of candidate discoveries, ranked from highest to lowest probability of being a real asteroid based on astrometric orbit-fit residuals and other considerations.

Prompt Products Database Solar System Tables
============================================

.. image:: images/LSST-Solar-System-Processing-Infographic-PPDB.png

The PPDB's Solar System tables are rebuilt every day.
They fall into three groups:

- **Rubin's observations and objects**: ``SSObservation`` and ``SSObject``.
  These are built only from Rubin observations that the Minor Planet Center (MPC) has accepted.
  Besides alert production, they include observations from reprocessed earlier data (e.g., DP1 and DP2).
- **Predictions near Rubin detections**: ``NearbySSO``.
  It is built from every ``DiaSource`` in the PPDB and the current MPC orbits.
- **MPC reference tables**: ``mpc_orbits``, ``current_identifications``, and ``numbered_identifications``.
  These are daily, unmodified copies of tables from the `MPC database <https://data.minorplanetcenter.net/mpcops/documentation/replicated-tables-schema/>`_ and keep the MPC's definitions.

The PPDB's ``DiaSource`` table holds the alerts from 2025 October 24 onwards.

.. list-table::
   :header-rows: 1
   :widths: 30 70

   * - Table
     - Contents
   * - :doc:`SSObservation </products/catalogs/ss_observation>`
     - One row per Rubin observation accepted by the MPC, with the measurement it was submitted from (a ``DiaSource`` or a ``Source``) and the object's predicted position.
   * - :doc:`SSObject </products/catalogs/ss_object>`
     - One row per object with at least one accepted Rubin observation: observation counts, per-band phase-curve fits, and orbit-derived quantities.
   * - :doc:`NearbySSO </products/catalogs/nearby_sso>`
     - For each ``DiaSource``, the nearest known object predicted within 5 arcseconds (15 for comets); also predictions that matched no ``DiaSource``.
   * - :doc:`mpc_orbits </products/catalogs/mpc_orbits>`
     - The MPC's orbit catalog: orbital elements and fit information.
   * - :doc:`current_identifications </products/catalogs/current_identifications>`
     - The MPC's mapping between designations of the same object (primary and secondary provisional designations).
   * - :doc:`numbered_identifications </products/catalogs/numbered_identifications>`
     - The MPC's mapping between permanent numbers and primary provisional designations.

The tables join on these keys:

- ``ssObjectId``: ``SSObject``, ``SSObservation``, and ``NearbySSO``.
- ``designation``: the same three tables and ``mpc_orbits``.
- ``diaSourceId``: ``NearbySSO`` and ``DiaSource``.

The Solar System tables and DiaSource are separate datasets
------------------------------------------------------------

Think of the Solar System tables and the PPDB's ``DiaSource`` table as two separate datasets.
You should almost never need to join them.

``SSObservation`` has a ``diaSourceId`` column, but its observations come from more than the PPDB's alert processing.
They also come from reprocessings of earlier data (e.g., DP1 and DP2) and from direct-image ``Sources``.
The ``processing`` column says which processing each observation comes from.
Only rows with ``processing = 'AP-DS'`` (alert production, difference images) refer to the PPDB's ``DiaSource`` table.
In other rows, ``diaSourceId`` identifies a source of a different processing; it is unique only together with ``processing``.

Even for ``AP-DS`` rows, the join isn't needed: each ``SSObservation`` row already contains the full source record of the observation.

The exception is ``NearbySSO``.
It is built on the PPDB's ``DiaSources``, so joining it to ``DiaSource`` on ``diaSourceId`` is how you get the measurements of the detections it lists.


Which table should I use?
-------------------------

.. list-table::
   :header-rows: 1
   :widths: 50 50

   * - To find...
     - Use
   * - Rubin's observations of an object
     - ``SSObservation``
   * - An object's absolute magnitudes, phase-curve slopes, MOID, or Tisserand parameter
     - ``SSObject``
   * - An object's orbit
     - ``mpc_orbits``
   * - Known objects that could explain a ``DiaSource``
     - ``NearbySSO``
   * - Where a known object should have been seen but wasn't
     - ``NearbySSO`` (rows with a NULL ``diaSourceId``)
   * - The primary designation of a numbered object, or of an alternate designation
     - ``numbered_identifications`` or ``current_identifications``

The Solar System tables identify objects by their primary provisional designation (``designation``).
To look up an object by its number, translate it first with ``numbered_identifications``, as in the second example below; use ``current_identifications`` the same way for an alternate designation.


Example queries
---------------

All Rubin observations of an object, by its designation:

.. code-block:: sql

   SELECT midpointMjdTai, band, ra, dec, psfMag, psfMagErr
   FROM ppdb.SSObservation
   WHERE designation = '1932 OA'
   ORDER BY midpointMjdTai

The same object, (1246), looked up by its number:

.. code-block:: sql

   SELECT o.midpointMjdTai, o.band, o.ra, o.dec, o.psfMag, o.psfMagErr, o.phaseAngle
   FROM ppdb.SSObservation AS o
   JOIN ppdb.numbered_identifications AS n
     ON o.designation = n.unpacked_primary_provisional_designation
   WHERE n.permid = '1246'
   ORDER BY o.midpointMjdTai

The object's orbit (``epoch_mjd`` is in TT):

.. code-block:: sql

   SELECT designation, a, e, i, q, epoch_mjd, h
   FROM ppdb.mpc_orbits
   WHERE designation = '1932 OA'

The ten objects Rubin has observed most:

.. code-block:: sql

   SELECT TOP 10 designation, nObs, arc, firstObservationMjdTai
   FROM ppdb.SSObject
   ORDER BY nObs DESC

r-band phase-curve parameters from at least 20 observations, where the slope was fitted (when the fit falls back to G12 = 0.5, ``r_G12Err`` is NULL):

.. code-block:: sql

   SELECT designation, r_H, r_HErr, r_G12, r_G12Err, r_nObsUsed,
          r_phaseAngleMin, r_phaseAngleMax
   FROM ppdb.SSObject
   WHERE r_G12Err IS NOT NULL AND r_nObsUsed >= 20

All observations of objects on Jupiter-family comet (JFC) orbits, selected by their Tisserand parameter with respect to Jupiter, 2 < T\ :sub:`J` < 3 (``tisserand_J`` in ``SSObject``, computed from the MPC orbit).
This simple cut also selects the Jupiter Trojans (semi-major axis near 5.2 au); join ``mpc_orbits`` on ``designation`` and cut on ``a`` or ``q`` to exclude them.

.. code-block:: sql

   SELECT o.designation, o.midpointMjdTai, o.band, o.ra, o.dec, o.psfMag, o.psfMagErr,
          s.tisserand_J
   FROM ppdb.SSObservation AS o
   JOIN ppdb.SSObject AS s ON o.ssObjectId = s.ssObjectId
   WHERE s.tisserand_J BETWEEN 2 AND 3
   ORDER BY o.designation, o.midpointMjdTai

The ``DiaSources`` of one visit whose nearest predicted object lies within three times the prediction's uncertainty (``ephOffset`` is in arcseconds; ``ephRaErr`` and ``ephDecErr`` are in degrees):

.. code-block:: sql

   SELECT n.diaSourceId, n.designation, n.ephOffset,
          n.ephRaErr * 3600 AS ephRaErr_arcsec, n.ephDecErr * 3600 AS ephDecErr_arcsec
   FROM ppdb.NearbySSO AS n
   WHERE n.visit = 2026011200081
     AND n.diaSourceId IS NOT NULL
     AND n.diaDistanceRank = 1
     AND n.ephOffset < 3 * 3600 * SQRT(n.ephRaErr * n.ephRaErr + n.ephDecErr * n.ephDecErr)

Visits where an object was predicted but no ``DiaSource`` matched it (non-detections):

.. code-block:: sql

   SELECT n.visit, n.detector, n.ephRa, n.ephDec, n.ephVmag
   FROM ppdb.NearbySSO AS n
   WHERE n.designation = '2006 SH230' AND n.diaSourceId IS NULL


Things to know
--------------

- **SSObservation and SSObject lag the alerts.**
  An observation appears only after the MPC has accepted and published it, days to months after it was made.
  Single-night detections not yet linked to an object (singletons) are currently not being submitted to the MPC, at the MPC's request.
- **NearbySSO shows proximity, not identification.**
  The ``DiaSource`` may be a different object or an artifact, especially when ``ephOffset`` is large compared with the prediction's uncertainty.
  Confirmed observations are in ``SSObservation``.
- **The Solar System tables and DiaSource are separate datasets.**
  ``SSObservation`` contains the full measurement of every observation, including many not in the PPDB's ``DiaSource`` table; you should almost never need to join the two.
  ``NearbySSO`` is the exception: it is built on the PPDB's ``DiaSources``.
- **The ssObjectId is an opaque identifier.**
  It does not change when an object is numbered.
  It does change if the MPC changes the object's primary provisional designation, for example when two designations turn out to be the same object.
  For references that must last, record the designation or number.
- **The tables are rebuilt daily.**
  New observations, discoveries, orbits, and designation changes appear the next day, so query results can change from day to day.
- **Check the phase-curve fits.**
  Where ``{band}_slope_fit_failed`` is true, G12 was fixed at 0.5 and only H was fitted; see :doc:`SSObject </products/catalogs/ss_object>`.

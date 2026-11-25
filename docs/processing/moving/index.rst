.. _moving:

#############################
Solar System Processing (SSP)
#############################

Solar System Processing includes both the association of known Solar System Objects (SSOs) during nighttime observations (see the :doc:`/processing/association/index` page) as well as the discovery of new SSOs by linking unassociated ``DiaSources`` during daytime processing.
Rubin reports its SSO observations and discoveries to the Minor Planet Center (MPC); single-night detections of objects that cannot yet be identified (singletons) are currently held at the MPC's request.
The MPC identifies the observations, assigns designations, and computes the orbits of all known objects; Rubin derives per-object quantities (e.g., per-band absolute magnitudes) from its own MPC-accepted observations.


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

The prompt products database (PPDB) delivers Rubin's MPC-accepted observations of small bodies, each with the difference image detection (``DiaSource``) or direct image detection (``Source``) it was measured from (``SSObservation``), Rubin-derived object-level photometry and summary properties (``SSObject``), and the known objects predicted near each ``DiaSource`` (``NearbySSO``); it is updated daily.
The ``DiaSource`` catalog is an alert product of Prompt Processing; the PPDB holds the alerts from 2025 October 24 onwards.
The ``SSObservation`` and ``SSObject`` catalogs are rebuilt daily from Rubin's observations accepted by the MPC, which include observations from reprocessings of earlier data (e.g., DP1 and DP2) as well as from alert production.
The ``NearbySSO`` catalog is rebuilt daily from all PPDB ``DiaSources`` and the current MPC orbits. The PPDB also includes daily snapshots of three tables from the `Minor Planet Center (MPC) PostgreSQL database <https://data.minorplanetcenter.net/mpcops/documentation/replicated-tables-schema/>`_ (see :doc:`PPDB catalogs </products/catalogs/index>` for details):

.. list-table::
   :header-rows: 1
   :widths: 30 70

   * - Table
     - Purpose
   * - :doc:`SSObservation </products/catalogs/ss_observation>`
     - One row per MPC record of each Rubin-submitted and MPC-accepted observation, joined to the full measurement from which it was submitted (a difference image ``DiaSource`` or a direct image ``Source``) and its ephemeris from ``mpc_orbits``, independent of the PPDB ``DiaSource`` table.
   * - :doc:`SSObject </products/catalogs/ss_object>`
     - One row per object with at least one MPC-accepted Rubin observation, built only from those observations (``SSObservation``), with observation summaries, per-band absolute-magnitude fits, and selected orbit-derived quantities.
   * - :doc:`NearbySSO </products/catalogs/nearby_sso>`
     - For each ``DiaSource``, the nearest known Solar System object predicted within 5 arcseconds (15 arcseconds for comets); and, with a NULL ``diaSourceId``, each prediction on (or within 30 arcseconds of) a detector that matched no ``DiaSource``.
   * - :doc:`mpc_orbits </products/catalogs/mpc_orbits>`
     - An MPC orbit-catalog snapshot containing orbital elements and associated fit information.
   * - :doc:`current_identifications </products/catalogs/current_identifications>`
     - An MPC mapping of primary and secondary provisional designations for the same physical object.
   * - :doc:`numbered_identifications </products/catalogs/numbered_identifications>`
     - An MPC mapping between permanent numbers and primary provisional designations.

**Join Keys:** The ``SSObject``, ``SSObservation``, and ``NearbySSO`` catalogs can be joined using the ``ssObjectId``. The ``SSObject``, ``SSObservation``, and ``mpc_orbits`` catalogs can be joined using the ``designation``. The ``DiaSource`` catalog can be joined with the ``NearbySSO`` table using the ``diaSourceId``. **NOTE:** The ``SSObservation`` table contains the ``DiaSource``- or ``Source``-like columns for the difference image detection or direct image detection from which the Rubin observation was submitted to the MPC and is independent of the PPDB ``DiaSource`` table; there should thus never be a need to join the ``SSObservation`` and ``DiaSource`` tables.

To retrieve the observations of Solar System objects that Rubin has submitted to and been accepted by the Minor Planet Center (MPC), use the ``SSObservation`` table. For orbit information, use the ``mpc_orbits`` table, and for per-object properties, such as per-band absolute magnitudes, use the ``SSObject`` table. These three tables can be joined using the Solar System object ``designation``. To find which known objects were predicted near ``DiaSources``, and how close, query ``NearbySSO``. To search for non-detections of known Solar System objects in prompt processing, select the ``NearbySSO`` rows with a NULL ``diaSourceId``.


The three MPC tables retain the upstream MPC field semantics rather than being recomputed from Rubin observations.
The delivered ``mpc_orbits``, ``current_identifications``, and ``numbered_identifications`` reference products are daily snapshots of the `MPC PostgreSQL tables <https://data.minorplanetcenter.net/mpcops/documentation/replicated-tables-schema/>`_.


Which table should I use?
-------------------------

.. list-table::
   :header-rows: 1
   :widths: 50 50

   * - To find...
     - Use
   * - Rubin's observations of an object (astrometry, photometry, observing geometry)
     - ``SSObservation``
   * - Per-object properties (per-band absolute magnitudes and phase-curve slopes, Earth MOID, Tisserand parameter)
     - ``SSObject``
   * - An object's orbit
     - ``mpc_orbits``
   * - Which known objects were predicted near a ``DiaSource``, or predicted positions where no ``DiaSource`` was detected
     - ``NearbySSO``
   * - The primary provisional designation for a permanent number or an alternate designation
     - ``numbered_identifications`` or ``current_identifications``

The Solar System tables are keyed on the primary provisional designation (``designation``).
To look up an object by its number or by another designation, first translate it with ``numbered_identifications`` or ``current_identifications``, as in the examples below.


Example queries
---------------

All Rubin observations of asteroid (1246):

.. code-block:: sql

   SELECT o.midpointMjdTai, o.band, o.ra, o.dec, o.psfMag, o.psfMagErr, o.phaseAngle
   FROM ppdb.SSObservation AS o
   JOIN ppdb.numbered_identifications AS n
     ON o.designation = n.unpacked_primary_provisional_designation
   WHERE n.permid = '1246'
   ORDER BY o.midpointMjdTai

The same for an object known by an alternate (secondary) designation, here 2025 PL160:

.. code-block:: sql

   SELECT o.midpointMjdTai, o.band, o.ra, o.dec, o.psfMag, o.psfMagErr
   FROM ppdb.SSObservation AS o
   JOIN ppdb.current_identifications AS c
     ON o.designation = c.unpacked_primary_provisional_designation
   WHERE c.unpacked_secondary_provisional_designation = '2025 PL160'
   ORDER BY o.midpointMjdTai

r-band phase-curve parameters, only where the slope was fitted (a fit that fell back to G12 = 0.5 has a NULL ``r_G12Err``), from at least 20 observations:

.. code-block:: sql

   SELECT designation, r_H, r_HErr, r_G12, r_G12Err, r_nObsUsed,
          r_phaseAngleMin, r_phaseAngleMax
   FROM ppdb.SSObject
   WHERE r_G12Err IS NOT NULL AND r_nObsUsed >= 20

``DiaSources`` whose nearest predicted known object lies within three times the prediction's positional uncertainty (``ephOffset`` is in arcseconds, ``ephRaErr`` and ``ephDecErr`` in degrees):

.. code-block:: sql

   SELECT n.diaSourceId, n.designation, n.ephOffset,
          n.ephRaErr * 3600 AS ephRaErr_arcsec, n.ephDecErr * 3600 AS ephDecErr_arcsec
   FROM ppdb.NearbySSO AS n
   WHERE n.diaSourceId IS NOT NULL
     AND n.diaDistanceRank = 1
     AND n.ephOffset < 3 * 3600 * SQRT(n.ephRaErr * n.ephRaErr + n.ephDecErr * n.ephDecErr)

Predicted positions of an object in visits where no ``DiaSource`` matched it (non-detections):

.. code-block:: sql

   SELECT n.visit, n.detector, n.ephRa, n.ephDec, n.ephVmag
   FROM ppdb.NearbySSO AS n
   WHERE n.designation = '2006 SH230' AND n.diaSourceId IS NULL


Things to know
--------------

- **SSObservation and SSObject lag the alerts.** They contain only observations the MPC has accepted and published, so an observation appears days to months after it was made. Single-night detections that cannot yet be identified with an object (singletons) are currently held at the MPC's request.
- **NearbySSO is a statement about proximity, not identification.** A row says that a known object was predicted near a ``DiaSource``; the ``DiaSource`` may be a different object or an artifact, particularly when ``ephOffset`` is large compared with the prediction's uncertainty. Observations confirmed by the MPC are in ``SSObservation``.
- **The ssObjectId is an opaque identifier.** It stays the same when an object is numbered, but changes if the MPC changes the object's primary provisional designation (for example, when it finds that two designations are the same object); the object's rows then appear under the new ``ssObjectId``. For references that must last, record the designation or number.
- **The tables are rebuilt daily.** Newly accepted observations, new discoveries, orbit updates, and designation changes all appear the next day, so query results can change from day to day.
- **Check the phase-curve fits.** Where ``{band}_slope_fit_failed`` is true, G12 was fixed at 0.5 and only H was fitted; see :doc:`SSObject </products/catalogs/ss_object>`.

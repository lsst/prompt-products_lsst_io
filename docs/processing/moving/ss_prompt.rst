.. _moving-prompt:

##############################
Solar System prompt processing
##############################

.. image:: images/LSST-Solar-System-Processing-Infographic.png

The goal of the nightly Solar System Processing (SSP) pipeline is to link (identify) previously unknown ``SSObjects``, given an additional night of observing, and report the discoveries to the `Minor Planet Center <https://minorplanetcenter.net>`_ (MPC), as well as to compute physical (e.g., absolute magnitudes) and other auxiliary properties (e.g., predicted apparent magnitudes and coordinates in various coordinate systems) for known Solar System objects and their LSST observations.

The majority of the pipeline's processing occurs in daytime, after a night of observing.
The pipeline will deliver daily data products for Solar System objects in the form of catalogs: ``SSObject``, ``SSObservation``, ``NearbySSO``, ``mpc_orbits``, ``current_identifications``, and ``numbered_identifications``, which are described in the :doc:`/products/catalogs/index` documentation.
The SSP pipeline is illustrated in the infographic provided above.
For more information on the moving object tracklet linking and orbit fitting algorithm, see :doc:`/processing/moving/ss_linking`.


The 24 hour processing cycle
============================

During operations, the pipeline will consist of the following steps that will repeat every 24 hours.

During nightly observing:

1. Alerts are issued within 60 seconds for all ``DiaSources`` with a signal-to-noise ratio (SNR) of at least 5. The Alert Production pipeline matches each ``DiaSource`` against the predicted positions of known Solar System objects; when it finds a match, the alert records the object's designation. See the :doc:`/processing/association/index` page for details.

During the day following nightly observing:

2. The previous night's ``DiaSources`` (SNR of at least 5) that were not matched to a known object go to the HelioLinC3D linking pipeline. It groups detections of the same object within a night into tracklets, then links tracklets from different nights (at least three within 15 days) into candidate objects on Keplerian orbits. See :doc:`/processing/moving/ss_linking` for the algorithm.

3. Observations of known objects and new discoveries are submitted to the Minor Planet Center (MPC) in its ADES format. Single-night detections not yet linked to an object (singletons) are generally not being submitted to the MPC at present, at the MPC's request; the exceptions are mainly distant objects such as TNOs.

During the day before the coming night’s observing:

4. A snapshot of the MPC database is taken. It includes:

   - the MPC's orbits and designations, which reflect the discoveries and observations of all programs (delivered as ``mpc_orbits``, ``current_identifications``, and ``numbered_identifications``);
   - the MPC's record of Rubin's accepted observations (observatory code X05).

5. The Solar System tables are rebuilt from this snapshot:

   - ``SSObservation``: every Rubin observation accepted by the MPC, with the measurement it was submitted from;
   - ``SSObject``: per-object quantities (phase-curve fits, extendedness summaries, MOID, Tisserand parameter) recomputed from those observations and the current orbits;
   - ``NearbySSO``: the known objects predicted near each ``DiaSource``, and predictions that matched no ``DiaSource``.

   Predicted positions and magnitudes are recomputed from the current orbits.

6. The Solar System tables are released in the PPDB.

Production of Solar System PPDB tables
======================================

.. image:: images/LSST-Solar-System-Processing-Infographic-PPDB-production.png

The infographic above shows how the Solar System PPDB tables are produced (step 5 above).

Raw LSSTCam images are processed by Prompt Processing, which performs difference imaging and issues alerts.
Its detections are kept in the Alert Production Database (APDB; schema: `<https://sdm-schemas.lsst.io/apdb.html>`_) and accumulated in the PPDB (``DiaSource``, ``DiaObject``, and ``DiaForcedSource``).
See the :doc:`/processing/association/index` page for the association of known Solar System objects during nighttime observations.

Each day, the Solar System tables are built from:

- the PPDB ``DiaSources`` (for ``NearbySSO``) and the measurements Rubin submitted to the MPC (for ``SSObservation``), which include measurements from reprocessed data (e.g., DP1 and DP2) and direct image ``Sources``;
- a snapshot of the MPC database: the orbit catalog, the designation tables, and the MPC's record of Rubin's accepted observations;
- the geometry of each visit and detector (WCS and detector footprints), used to place predicted positions on the focal plane;
- each source's mid-exposure time corrected for the shutter's motion across the focal plane (a difference of up to about 0.24 s from the visit's midpoint).

Each MPC-accepted Rubin observation is matched to the measurement it was submitted from, ephemerides of known objects are computed from the MPC orbits with the ASSIST integrator, and the tables are checked for internal consistency before they are released.
Times in ``SSObservation`` and ``NearbySSO`` are corrected for the shutter's motion where a correction is available; ``DiaSource`` times, and the times in Rubin's past MPC submissions, are the visit's midpoint.

The Solar System PPDB tables include ``SSObservation``, ``SSObject``, ``NearbySSO``, ``mpc_orbits``, ``current_identifications``, and ``numbered_identifications``. For more information on each catalog, see the :doc:`/products/catalogs/index` pages.

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

1. Known Solar System objects are associated with difference image detections in real-time. Alerts are produced within 60 seconds for all signal-to-noise ratio (SNR)>=5 ``DiaSources``. The Alert Production pipeline attempts association of ``DiaSources`` with known Solar System objects in real-time, and if a match is found then the alert records the object's designation. For details on Solar System object association, see the :doc:`/processing/association/index` page.

During the day following nightly observing:

2. ``DiaSources`` (SNR>=5) detected on the previous night that are not associated with a known Solar System object are passed to the HelioLinC3D moving object linking pipeline. It builds tracklets (detections of the same object within a single night) and links tracklets from different nights (at least three tracklets within 15 days) into candidates consistent with a single Keplerian orbit around the Sun. For more information on this algorithm, see :doc:`/processing/moving/ss_linking`.

3. Measurements of known objects and new discoveries are submitted to the Minor Planet Center (MPC) in the standard ADES format. Single-night detections that cannot yet be identified with an object (singletons) are currently held at the MPC's request.

During the day before the coming night’s observing:

4. A snapshot of the Minor Planet Center (MPC) database is taken: its orbit catalog and designation tables (delivered as ``mpc_orbits``, ``current_identifications``, and ``numbered_identifications``), which reflect the discoveries and observations reported by all programs, and the MPC's record of Rubin's accepted observations (observatory code X05).

5. The daily data product catalogs (PPDB) are rebuilt from this snapshot. The ``SSObservation`` catalog holds all of Rubin's observations accepted by the MPC, each with the measurement it was submitted from, independent of the PPDB ``DiaSource`` table. The ``NearbySSO`` catalog holds the nearest predicted known Solar System object for each ``DiaSource``, and the predictions on (or within 30" of) a detector that matched no ``DiaSource`` (with a NULL ``diaSourceId``). The per-object quantities in ``SSObject`` (per-band absolute magnitudes and phase-curve slopes, extendedness summaries, and orbit-derived quantities such as the Earth MOID and the Tisserand parameter) are recomputed, and the predicted positions and magnitudes in ``SSObservation`` and ``NearbySSO`` are recomputed from the current orbits.

6. The Solar System daily data products (``SSObject``, ``SSObservation``, ``NearbySSO``, ``mpc_orbits``, ``current_identifications``, and ``numbered_identifications`` tables) are released.

7. Precovery linking is attempted for all ``SSObjects`` whose orbits were updated in the above process (or are new). Where successful, newly discovered observations are queued up for submission to the Minor Planet Center.

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

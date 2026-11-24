.. _moving-prompt:

##############################
Solar System prompt processing
##############################

.. image:: images/LSST-Solar-System-Processing-Infographic.png

The goal of the nightly Solar System Processing (SSP) pipeline is to link (identify) previously unknown ``SSObjects``, given an additional night of observing, and report the discoveries to the `Minor Planet Center <https://minorplanetcenter.net>`_ (MPC), as well as to compute physical (e.g., absolute magnitudes) and other auxiliary properties (e.g., predicted apparent magnitudes and coordinates in various coordinate systems) for known Solar System objects and their LSST observations.

The majority of the pipeline's processing occurs in daytime, after a night of observing.
The pipeline will deliver daily data products for Solar System objects in the form of catalogs: ``SSObject``, ``SSObservation``, ``mpc_orbits``, ``current_identifications``, ``numbered_identifications``, ``NearbySSO``, and ``DiaSource``, which are described in the :doc:`/products/catalogs/index` documentation.
The SSP pipeline is illustrated in the infographic provided above.
For more information on the moving object tracklet linking and orbit fitting algorithm, see :doc:`/processing/moving/ss_linking`.


The 24 hour processing cycle
============================

During operations, the pipeline will consist of the following steps that will repeat every 24 hours.

During nightly observing:

1. Known Solar System objects are associated with difference image detections in real-time. Alerts are produced within 60 seconds for all signal-to-noise ratio (SNR)>=5 ``DiaSources``. The Alert Production pipeline attempts association of ``DiaSources`` with known Solar System objects in real-time, and if a match is found then the alert includes the corresponding ``SSSource`` catalog entry. For details on Solar System object association, see the :doc:`/processing/association/index` page.

During the day following nightly observing:

2. All ``DiaSources`` detected on the previous night that have not been matched at a high confidence level (SNR>=5) to a known Object, ``DiaObject``, ``SSObject``, or an artifact, are analyzed by the HelioLinC3D moving object linking algorithm for potential pairs that form tracklets (consisting of detections in three pairs of images for a given visit within 15 days) that are consistent with being on the same Keplerian orbit around the Sun. For more information on this algorithm, see :doc:`/processing/moving/ss_linking`.

3. Measurements of known objects and new discoveries are submitted to the Minor Planet Center (MPC) using the standard data-exchange protocols (e.g., the ADES format). The measurements of all ``DiaSources`` detected on the previous night that have been matched at a high level of confidence (SNR>=5) to a known ``SSObject`` are also submitted to the MPC.

During the day before the coming night’s observing:

4. The most up-to-date ``mpc_orbits``, ``current_identifications``, and ``numbered_identifications`` catalogs are downloaded from the Minor Planet Center (MPC) and ingested into the Prompt Products database to obtain all previously submitted LSST discoveries and detections as well as discoveries and detections by other contemporaneous programs made during the past 24 hours.

5. The daily data product catalogs (PPDB) are updated to include the new Solar System object discoveries from the MPC. The ``SSObservation`` catalog is updated with all observations submitted to and accepted by the MPC for all corresponding ``SSObjects``, independent of the PPDB ``DiaSource`` table. The ``NearbySSO`` catalog is updated with the nearest predicted known Solar System object for each ``DiaSource``; the ``NearbySSO`` catalog also includes the nearest objects predicted to land on a detector (or within 30" of one) but match no ``DiaSource`` (with ``DiaSourceId`` = NULL). In addition, the physical properties (e.g., absolute magnitudes, predicted apparent magnitudes, extendedness estimates, and light curve characteristics), as defined by the orbit catalog of all known ``SSObjects``, including new discoveries ingested from the MPC, are recomputed. Updated data are entered into the relevant tables.

6. The Solar System daily data products (``SSObject``, ``SSObservation``, ``mpc_orbits``, ``current_identifications``, ``numbered_identifications``, ``NearbySSO``, and ``DiaSource`` tables) are released.

7. Precovery linking is attempted for all ``SSObjects`` whose orbits were updated in the above process (or are new). Where successful, newly discovered observations are queued up for submission to the Minor Planet Center.

Production of Solar System PPDB tables
======================================

.. image:: images/LSST-Solar-System-Processing-Infographic-PPDB-production.png

The above infographic illustrates the process by which the Solar System PPDB tables are produced (see step 5 above).

The LSST Camera raw images are input the prompt processing pipeline to perform difference imaging and generate alerts.

The alerts are kept in the Alert Production Database (APDB; schema: `<https://sdm-schemas.lsst.io/apdb.html>`_).
See the :doc:`/processing/association/index` page for more information on the association of known Solar System Objects (SSOs) during nighttime observations.

The Prompt Products Database (PPDB) contains the same data (and more) as the APDB accumulated and queryable. This includes the ``DiaSource``, ``DiaObject``, ``DiaForcedSource``, and Solar System tables.

The ``DiaSource`` catalog from the PPDB is ingested into the ssp-tools build along with the shutter timing information, visit table, and MPC tables.
``ssp-tools`` then matches each MPC-accepted Rubin observation to its ``DiaSource`` measurement, computes ephemerides for all known Solar System objects using the ASSIST integrator, and performs internal checks.
Shutter timing corrections are included in the ``SSObservation`` and ``NearbySSO`` tables; the ``DiaSource`` and past MPC submissions use the visit midpoint.
The daily Solar System data products (i.e., the Solar System PPDB tables) are then generated.

The Solar System PPDB tables include ``SSObservation``, ``SSObject``, ``NearbySSO``, ``mpc_orbits``, ``current_identifications``, and ``numbered_identifications``. For more information on each catalog, see the :doc:`/products/catalogs/index` pages.

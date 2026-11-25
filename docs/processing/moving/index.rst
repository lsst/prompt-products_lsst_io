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

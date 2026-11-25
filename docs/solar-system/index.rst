.. _solar-system:

###################################
Solar System science with the PPDB
###################################

This guide is for astronomers who work on asteroids, comets, and other small bodies, and want to use Rubin's data.
It assumes you know the Minor Planet Center (MPC), designations, absolute magnitudes and phase curves, and a little SQL.
It does not assume you know anything about Rubin.

.. toctree::
   :maxdepth: 1
   :hidden:

   concepts
   quality
   tables
   recipes
   caveats
   production


Your first query
================

The Solar System tables live in the Prompt Products Database (PPDB), which you query with ADQL, a dialect of SQL, through the Rubin Science Platform (RSP).

1. **Get an RSP account.** Follow the `instructions to sign up <https://rsp.lsst.io/guides/getting-started/get-an-account.html>`_. RSP access requires Rubin data rights; see :doc:`/access/index`.
2. **Open a query interface.** Use the Portal's ADQL search or a notebook in the Notebook Aspect (see :doc:`recipes` for the Python version), and choose the PPDB TAP service.
3. **Run a query.** This one returns every Rubin observation of Pluto, (134340):

.. code-block:: sql

   SELECT o.midpointMjdTai, o.band, o.ra, o.dec, o.psfMag, o.psfMagErr
   FROM ppdb.SSObservation AS o
   JOIN ppdb.numbered_identifications AS n
     ON o.designation = n.unpacked_primary_provisional_designation
   WHERE n.permid = '134340'
   ORDER BY o.midpointMjdTai

You get a table of times, filters, positions, and magnitudes: one row for each observation of Pluto that Rubin reported and the MPC accepted.


The idea in four sentences
==========================

- ``SSObservation`` holds every Rubin observation of a Solar System object that the MPC has accepted, with the full Rubin measurement behind it.
- ``SSObject`` summarizes those observations, one row per object: observation counts, per-band phase-curve fits, and a few orbit-derived quantities.
- ``NearbySSO`` lists the known objects predicted near each Rubin detection; it tells you what an object *could* be, not what it *is*.
- ``mpc_orbits``, ``current_identifications``, and ``numbered_identifications`` are daily copies of the MPC's orbit and designation tables, so you can join orbits to Rubin data in one query.

.. image:: /processing/moving/images/LSST-Solar-System-Processing-Infographic-PPDB.png


Two things to know before you start
===================================

**The tables are rebuilt every day.**
They follow the MPC: new observations, new discoveries, improved orbits, and changed designations all appear the next day.
That keeps them current, but it also means that the same query can return different results tomorrow.
If you use the results in a paper, save the table you queried and note the date.

**The PPDB is the current view; data releases are the reference catalogs.**
The PPDB gives you Rubin's best current knowledge of the Solar System, updated daily.
Rubin's data releases provide versioned catalogs, processed homogeneously and with characterized systematics and selection.
Use the PPDB to work with the latest data on an object or a population.
Use a data release when you need a fixed, homogeneous catalog, for example to measure a population's size distribution.


Where Rubin's small-body data live
==================================

The PPDB is one of four places to find Rubin's Solar System data.

.. list-table::
   :header-rows: 1
   :widths: 22 46 32

   * - Where
     - What
     - Access
   * - `Minor Planet Center <https://minorplanetcenter.net/>`_
     - All of Rubin's accepted astrometry, under observatory code X05.
     - Public.
   * - :doc:`Alerts </products/alerts/index>`
     - Every new detection in difference images, within minutes; detections near known objects carry the object's designation.
     - Public, through the :doc:`alert brokers </access/index>`.
   * - The PPDB (this guide)
     - Rubin's accepted observations with their full measurements, object summaries, predictions near every detection, and the MPC's orbits and designations.
     - The contents are public; querying them requires the RSP. See :doc:`/access/index`.
   * - Data releases
     - Versioned, homogeneously processed catalogs and images.
     - Data rights; see :doc:`/access/index`.


A few Rubin terms
=================

You will meet these words in column names and in the rest of this guide.

**Visit**: one exposure of the camera, identified by ``visit``. The camera has many CCDs, called detectors (``detector``).

**Template**: a deep image of a field, built from earlier visits.

**Difference image**: a visit with the template subtracted, so that only what changed remains: variable sources and moving objects.

**DiaSource**: a source detected and measured on a difference image. Most of Rubin's Solar System observations are DiaSources.

**Source**: a source detected and measured on the visit image itself. Rubin uses these for moving objects where no template exists yet.


Where to go next
================

- :doc:`concepts` explains how an observation gets into the tables and how to identify objects.
- :doc:`quality` covers uncertainties, flags, and what the tables do and don't contain.
- :doc:`tables` is a tour of each table and its most useful columns.
- :doc:`recipes` has copy-and-paste queries for common tasks.
- :doc:`caveats` lists the things that most often trip people up.
- :doc:`production` describes how the tables are made, for the curious.

For questions, post in the `Support category of the Rubin Community Forum <https://www.rubin.community/c/support/6>`_.
To cite the data, see :doc:`/cite`.

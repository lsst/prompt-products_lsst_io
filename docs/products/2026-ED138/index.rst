.. _2026-ED138:

####################################
2026 ED138 image stamps and catalogs
####################################

Release image cutouts and catalogs for Rubin Observatory LSSTCam data covering 2026 ED138.

**Proprietary data: for Rubin data rights holders only.**

This release contains proprietary Rubin Observatory LSST data.
Access and use are restricted to individuals holding Rubin data rights, under the `Rubin Data Policy <https://rubinobservatory.org/for-scientists/data-products/data-policy>`_ (RDO-013).

Publications using these data must meet the policy's co-authorship requirements for proprietary data and include the proprietary-data acknowledgement (DPOL-306), together with Rubin's standard funding acknowledgement; see `<https://citations.lsst.io/acknowledgements/index.html>`_.

The MPC submission in the ``ades/`` directory is public through the Minor Planet Center; its presence here does not make the rest of the release public.

Access
======

Copies of these images and catalogs are available on the Rubin Science Platform in ``/rubin/shared/datasets/2026-ED138`` as demonstrated in the 2026 ED138 prompt tutorial notebook (see the :doc:`/tutorials/index`).

Description
===========

The release of the Rubin Observatory LSSTCam data covering 2026 ED138, a trans-Neptunian object found in Rubin's 2026 observations, is available for Rubin data rights holders to study it with the original pixels and catalogs.
It holds every LSSTCam image the object falls on, the single-visit source catalogs around its track, the observation metadata needed to tell where Rubin looked and how deep, and the astrometry and photometry submitted to the Minor Planet Center (MPC).
All files are in standard Rubin formats: a Butler export that imports into any Butler repository, and Parquet tables.

Some uses: re-measuring positions and fluxes (e.g. with other PSF models or deblending choices), light curves and colors, searching the surrounding
visits for detections at other positions or for companions, checking the reported astrometry against the images, and studying how the pipelines
handled a faint moving source in a crowded field.

Contents
========

.. list-table:: 2026 ED138 release contents.
   :widths: auto
   :header-rows: 1
   :class: tight-table

   * - Path
     - Content
   * - ``butler_export/``
     - a Butler export (``export.yaml`` + files): 1,428 datasets in 63 runs, 24 GB
   * - ``butler_repo/``
     - a read-only Butler repository with the export already imported (SQLite registry; files are symlinks into ``butler_export/``)
   * - ``tutorial-2026-ed138.ipynb``
     - executed demo notebook: finds the 30 ADES observations in ``butler_repo/``, makes 20″ cutouts and plots them (source: ``noteboo
ks/ed138_release_demo.ipynb`` in rubin-tno-attribution, PR #75 @ 5e2d44c)
   * - ``manifest.csv``
     - every exported file: dataset type, run, data ID, path, bytes, sha256, set
   * - ``metadata/``
     - observation metadata as tables: ``<db>.<table>.parquet`` (rows) + ``<db>.<table>.sql`` (schema)
   * - ``ades/2026_ED138.psv``
     - the 30 observations of the object submitted to the Minor Planet Center (ADES PSV, tracklet ID ``RL021Fp``)

Coverage
--------

48 LSSTCam visits on 29 nights, 2026-03-04 to 2026-07-11, in which the object falls on a detector, plus visits of the same sky at other times.

Butler export
-------------

.. list-table:: Standard dataset types under their original run names.
   :widths: auto
   :header-rows: 1
   :class: tight-table

   * - Dataset type
     - Files
     - Size
   * - ``preliminary_visit_image``
     - 242
     - 7.3 GB
   * - ``preliminary_visit_image_background``
     - 213
     - 15.4 MB
   * - ``preliminary_visit_image_reprocessed_background``
     - 33
     - 2.9 MB
   * - ``raw``
     - 48
     - 0.9 GB
   * - ``singleFrameDetectAndMeasure_config``
     - 21
     - 1.8 MB
   * - ``single_visit_star_detector``
     - 434
     - 0.7 GB
   * - ``single_visit_star_footprints``
     - 96
     - 1.4 GB
   * - ``single_visit_star_reprocessed_footprints``
     - 60
     - 7.1 GB
   * - ``single_visit_star_reprocessed_unstandardized``
     - 256
     - 4.4 GB
   * - ``the_monster_20250219``
     - 25
     - 1.7 GB

Images
------

``preliminary_visit_image`` (+ background): Prompt Processing's calibrated single-visit images.

- ``set=core`` in the manifest: every detector the object is on, with its ``raw``;
- ``set=fixed``: every detector that, on another night, covers a sky position where the object was detected.

These show the same patch of sky without the object, for checking for static sources underneath it.

Catalogs
--------

``single_visit_star_*``: single-visit source catalogs of every detector containing one of the object's measured or predicted positions, from three processings:

- Daytime Alert Production reprocessing (``single_visit_star_reprocessed_*``, with each run's ``singleFrameDetectAndMeasure_config``);
- nightly validation processing (``single_visit_star_detector``);
- Prompt Processing (``single_visit_star_footprints``).

Reference catalog
-----------------

``the_monster_20250219`` shards along the track and around the detectors.

Dimension records
-----------------

(``visit``, ``exposure``, ``visit_detector_region``, ...) of every visit near the track, including visits with no dataset here.
Spatial and temporal queries (which visits covered a position, and when) therefore return every visit near the track, not only those with images in the release.

These are preliminary, early-operations processings, not a Rubin data release product: calibrations and pipelines are those in use at the time,
and differ between the three catalog processings.

Metadata
--------

``metadata/``: Observation metadata for the exposures within 4 deg of the object's field:

- ConsDB's ``cdb_lsstcam.exposure``, ``ccdexposure`` and ``ccdvisit1_quicklook``: exposures, per-detector sky polygons, and quicklook image quality and depth. The ``cdb_lsstcomcam`` tables are the LSSTComCam equivalents.
- ``ssp.Visit``: SSP's copy of the Prompt Products Database's ``Visit`` table, with SSP-added columns: visit boresights and times.

The ``.sql`` files give each table's schema as in its source database.

Use of this release
===================

On the RSP, open the ready-made repository ``butler_repo/`` directly (LSST Science Pipelines ``w_2026_41`` or later; it was built with ``w_2026_43``):

``python
| from lsst.daf.butler import Butler
| butler = Butler("/rubin/shared/datasets/2026-ED138/butler_repo", collections="2026-ED138")
| refs = butler.query_datasets("preliminary_visit_image", where="instrument = 'LSSTCam' AND visit = 2026030300796")
| image = butler.get(refs[0])
| ``

The repository is read-only. ``2026-ED138`` is a chained collection of all 63 runs, in order of priority: Daytime AP (26 runs), Prompt Processing (31), nightly validation (4), ``LSSTCam/raw/all``, and the reference catalog.
Some data IDs have datasets in more than one run: Prompt Processing and nightly validation both have ``single_visit_star_footprints`` for 44 detectors, and each Daytime AP run has its own ``singleFrameDetectAndMeasure_config``.
A find-first lookup through the chain returns the first of these, so Prompt Processing's catalog is preferred over nightly validation's.
To see them all, pass ``find_first=False`` or query the individual runs (``butler query-collections``).

Elsewhere, import the export into a new repository:

butler create REPO
| butler import REPO butler_export --export-file butler_export/export.yaml --transfer symlink

and use it in the same way, with ``collections="*"`` or the run names in place of ``2026-ED138``.

The ``metadata/`` tables are Parquet; any Parquet reader opens them, and the ``.sql`` schemas load them into a database (e.g.  ClickHouse or DuckDB) under their original table names.
``manifest.csv`` lists every file with its sha256 for verification.

Provenance
==========

Built at USDF on 2026-10-26 from the Butler repositories ``prompt_prep``, ``/repo/main`` and ``/repo/embargo``.
All datasets are copied as they are, with their original runs and dataset IDs.
Where those repositories hold different versions of a dimension record, the release has ``/repo/main``'s.
For example, ``visit_detector_region`` polygons differ for about 3% of visit-detectors.
The ``metadata/`` rows were extracted from ConsDB and SSP's copy of the PPDB in October 2026.

###############
Data processing
###############



A high-level overview of the Prompt Processing steps which generated the data products.

Prompt data processing uses the `LSST Science Pipelines <https://pipelines.lsst.io/v/weekly>`_.
|osprae_doi| [:download:`BibTeX </bib/osprae.bib>`].

.. important::

   Papers detailing the design and performance of the Alert Production and Solar System pipelines are in preparation.

Pipeline Overview
=================

A `pipeline definition <https://github.com/lsst-dm/prompt_processing/blob/main/pipelines/LSSTCam/ApPipe.yaml>`_ is deployed in Prompt Processing.
It imports pipelines from `ap_pipe <https://pipelines.lsst.io/v/weekly/modules/lsst.ap.pipe/index.html>`_.
The `pipeline yaml files <https://github.com/lsst/ap_pipe/tree/main/pipelines>`_ may be found in Github.

A graph of the pipeline stages and intermediate data products may be found `here <https://tigress-web.princeton.edu/~lkelvin/pipelines/current/ap_pipe/LSSTCam/ApPipe/pipeline_ap_pipe_LSSTCam_ApPipe.pdf>`_.
The resulting user-facing data products are described in the Data Products section.



Change log
==========

Unlike with the annual data releases which are associated with a given version of the
LSST Science Pipelines, Prompt Processing software payloads are updated frequently.

The `pipelines changelog <https://lsst-dm.github.io/lsst_git_changelog/weekly/index.html>`_ shows recent changes to the Science Pipelines.
Note that recently merged features may not immediately be deployed in Prompt Processing.  The deployed pipelines are typically updated at least weekly.

Prompt processing
=================

Describes the data processing framework.

.. toctree::
    :maxdepth: 1
    :glob:

    prompt-processing/index

Single frame
============

Processing and calibration of raw images into visit images.

.. toctree::
    :maxdepth: 1
    :glob:

    single-frame/index


Template generation
===================

Generates the template coadd images.

.. toctree::
    :maxdepth: 1
    :glob:

    templates/index


Difference image analysis
=========================

Runs image subtraction to generate difference images and associated detection catalogs.

.. toctree::
    :maxdepth: 1
    :glob:

    dia/index

Source association
==================

Associates detections with astrophysical objects.

.. toctree::
    :maxdepth: 1
    :glob:

    association/index

Filtering and Reliability Scoring
=================================

Removing false positives.

.. toctree::
    :maxdepth: 1
    :glob:

    filtering/index

Alert generation
================

Creates and distributes alert packets to brokers.

.. toctree::
    :maxdepth: 1
    :glob:

    alert-gen/index


.. _processing-moving:

Solar System processing
=======================

Links detected sources into moving objects and generates Solar System catalogs.

.. toctree::
    :maxdepth: 1
    :glob:

    moving/index

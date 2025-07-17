###############
Data processing
###############

.. important::

   **Placeholder.** Prompt data processing is under construction.


A high-level overview of the Prompt Processing steps which generated the data products.

All processing was done with the `LSST Science Pipelines <https://pipelines.lsst.io/>`_.


Change log
==========

Unlike with the annual data releases which are associated with a given version of the
LSST Science Pipelines, Prompt processing is constantly ongoing and updates happen
on-the-fly.

**Need to add link to a human-curated and auto-generated change log.**


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



Alert generation
================

Creating and distributing alert packets to brokers.

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

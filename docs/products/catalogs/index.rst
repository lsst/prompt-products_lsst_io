.. _catalogs:

########
Catalogs
########

.. important::

   **Placeholder.** Prompt catalogs are not yet available.


Tables of measurements for detections in difference images.

The Prompt catalogs together are referred to as the Alert Production Database (APDB).

The `APDB schema browser <https://sdm-schemas.lsst.io/apdb.html>`_ includes column descriptions for all tables.


Difference image analysis
=========================

Measurements on the difference and direct images at the locations of all variable or moving objects.

.. toctree::
    :maxdepth: 1
    :titlesonly:
    :glob:

    dia_object
    dia_source
    dia_forced_source


Moving objects
==============

Derived properties for moving objects detected in images.

.. toctree::
    :maxdepth: 1
    :titlesonly:
    :glob:

    ss_object
    ss_source
    mpcorb



Observations
============

Observational metadata for visits and detectors.

.. toctree::
    :maxdepth: 1
    :titlesonly:
    :glob:

    visit_table
    visit_detector_table

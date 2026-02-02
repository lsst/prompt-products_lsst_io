#######################
Observations and status
#######################

.. important::

   Rubin Observatory is still in an early operations phase.

The key objective is to improve the image quality and survey efficiency in order to start the ten-year LSST survey.
The amount of science-grade data collected during this early phase will vary.
For the general timeline during this phase, read more about the `Early Science Program <https://rubinobservatory.org/for-scientists/resources/early-science>`_.


LSSTCam
=======

All observations were obtained with the LSST Camera (LSSTCam).
Find a description, key numbers, filter bandpasses, and more in the `LSSTCam documentation <https://lsstcam.lsst.io/>`_.

|lsstcam_doi|


Survey strategy
===============

Information about the strategy and cadence for the Legacy Survey of Space and Time can be found at `survey-strategy.lsst.io <https://survey-strategy.lsst.io/>`_.


Nightly projection
------------------

The projected Rubin schedule can be obtained via ObsLocTAP `programatically <https://usdf-rsp.slac.stanford.edu/obsloctap/schedule?time=24>`_ or with the browser-based `viewer <https://usdf-rsp.slac.stanford.edu/obsloctap/static/viewer.html>`_.
A :ref:`tutorial notebook <tutorials-notebook>` for the Rubin Schedule Viewer is available.


Incremental templates
---------------------

During early Alert Production, templates (and thus alerts) only exist in the `Deep Drilling Fields <https://survey-strategy.lsst.io/baseline/ddf.html>`_.

As of the end of January 2026, templates had been deployed in Alert Production in the DDF listed in the table below.

.. list-table:: DDF templates
   :widths: 20 20 20
   :header-rows: 1
   :class: tight-table

   * - Name
     - Coordinates
     - Filters
   * - COSMOS
     - 150.11, 2.23
     - ugrizy
   * - ECDFS
     - 52.98, -28.12 deg
     - riz
   * - ELAIS-S1
     - 9.45, -44.02 deg
     - griz
   * - EDFS_a
     - 58.9, -49.32 deg
     - griz
   * - EDFS_b
     - 63.6, -47.6 deg
     - griz


Additional template images will be generated incrementally during the early science phase, and information about template regions will be added here.


Alert stream status
===================

A live dashboard showing alert processing over the last twelve hours is available at `ls.st/alert-stream-status <https://ls.st/alert-stream-status>`_.

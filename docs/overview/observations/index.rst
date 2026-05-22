############
Observations
############

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


Alert stream status
===================

A live dashboard showing alert processing over the last twelve hours is available at `ls.st/alert-stream-status <https://ls.st/alert-stream-status>`_.


Template coverage
=================

On May 21 2026, a significant expansion to the template coverage available to Alert Production was deployed, extending it beyond the original `Deep Drilling Fields <https://survey-strategy.lsst.io/baseline/ddf.html>`_ templates.

The best-covered subset of the templates generated as part of the forthcoming Data Preview 2 were selected.
This increased the total sky area for alert production by more than a factor of ten (depending on the band).

Current template coverage can be visualized with the `interactive alert production template coverage interface <https://s3df.slac.stanford.edu/people/ebellm/template_coverage/template_coverage.html>`_.
More detailed programatic access to template coverage under development.

.. list-table:: Current alert production coverage by filter.
   :widths: 20 20
   :header-rows: 1
   :class: tight-table

   * - Filter
     - Sky area (sq. deg)
   * - u
     - 53
   * - g
     - 211
   * - r
     - 905
   * - i
     - 1444
   * - z
     - 1197
   * - y
     - 25



Additional template images will be generated incrementally during the early science phase, and information about template regions will be added here.
Details are available in :doc:`/processing/templates/index`.


Survey strategy
===============

Information about the strategy and cadence for the Legacy Survey of Space and Time can be found at `survey-strategy.lsst.io <https://survey-strategy.lsst.io/>`_.

Nightly projection
------------------

The projected Rubin schedule can be obtained via ObsLocTAP with the browser-based `static viewer <https://usdf-rsp.slac.stanford.edu/obsloctap/static/viewer.html>`_
or `all-sky map interface <https://usdf-rsp.slac.stanford.edu/obsloctap/skymap>`_, and programmatically as demonstrated in the Commissioning :ref:`tutorials <tutorials-notebook>`.

Nightly reports
---------------

Automated per-night reports are publicly available as `scheduler viewer reports <https://s3df.slac.stanford.edu/data/rubin/sim-data/schedview/reports/>`_.
Static files of LSSTCam visits metadata are available in the Commissioning :ref:`tutorials <tutorials-notebook>`.
In the future, observational metadata will be available as queryable tables and documented under :doc:`/products/index`.





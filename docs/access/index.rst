###########
Data access
###########

Services and tools for accessing the alerts and the Prompt data products.

Alert brokers
=============

Software systems that ingest and process astronomical alerts from the LSST (and other surveys).

.. toctree::
    :maxdepth: 1
    :glob:

    brokers


Rubin Science Platform (RSP)
============================

.. important::

   The alert archive, PPDB, and prompt images are not yet available.


Past alerts and Prompt data products will be accessable via the RSP, as demonstrated in the :doc:`/tutorials/index`.

Alert archive
-------------

An archive of all past alerts will be available via the Rubin Science Platform.
This archive will be queryable by the ``DIA Object`` identifier.

Prompt Products Database (PPDB)
-------------------------------

The PPDB will be updated within 24 hours of alert distribution.
The same data that is streamed in alert packets is available for query and analysis via the RSP.

Prompt images
-------------

The promptly-processed visit and difference images will be available after an 80 hour embargo period via the RSP.

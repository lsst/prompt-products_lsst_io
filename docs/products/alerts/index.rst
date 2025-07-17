.. _alerts:

#############
Alert packets
#############

.. important::

   **Placeholder.** Alerts are not yet available.


Ascii files of measurements for detected difference-image sources.


Access
======

Alert packet contents are world public and have no proprietary period.

Brokers
-------

Alerts are delivered to :doc:`brokers/index` and accessed via individual brokers' web-based interfaces.

Archive
-------

Alerts are stored in an archive accessible via the Rubin Science Platform by their unique identifier number.
The alerts archive is not queryable; use the ``DiaObject`` and ``SSObject`` :doc:`catalogs/index` instead.


Schema
======

Alert packets are generated with Avro schema (JSON format).

Find the most recent version of the LSST `alert packet schema <https://github.com/lsst/alert_packet/tree/main/python/lsst/alert/packet/schema>`_/

Note that most broker users do not need to know the details of the packet schema
unless they are developing custom alert filters.


Description
===========

Each alert packet contains:

* unique identifier
* the ``DiaSource`` record that triggered the alert
* the associated ``DiaObject`` or ``SSObject`` record
* associated ``DiaSource`` and ``DiaForcedSource`` records from the past 12 months
* cutout stamps from the difference and template images

Processing
----------

The alerts packets are the result of :doc:`/processing/alert-gen/index`.

Tutorials
---------

TBD
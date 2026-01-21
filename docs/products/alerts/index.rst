.. _alerts:

#############
Alert packets
#############

Ascii files of measurements for 5-sigma source detections in difference images.

Schema: `Alert Production Database (APDB) <https://sdm-schemas.lsst.io/apdb.html>`_


Access
======

Alert packet contents are world public and have no proprietary period (see Section 3 of the `Rubin Data Policy <http://ls.st/rdo-013>`_).


Brokers
-------

Realtime access to alerts is available through community alert brokers.
Alerts are delivered to :doc:`/access/brokers` and accessed via individual brokers' interfaces.


Alert archive
-------------

As-transmitted alerts are stored in an archive accessible to Data Rights holders via the Rubin Science Platform.

.. important::

   The Alert Archive interface has not yet been released.


Description
===========

Each alert packet contains information triggered by a single ``DiaSource`` detected at 5 sigma in a difference image by by Prompt Processing.


Contents
--------

Each alert packet contains:

* unique identifier
* the ``DiaSource`` record that triggered the alert
* the associated ``DiaObject`` or ``SSObject`` record
* associated ``DiaSource`` and ``DiaForcedSource`` records from the past 12 months
* FITS-formatted cutout postage stamps from the science, difference, template images


Schema
------

A detailed description of the records contained within the alert packet is available at `sdm-schemas.lsst.io <https://sdm-schemas.lsst.io/apdb.html>`_.

Alert packets are serialized in the binary Apache Avro format.
The Avro schema is `alert packet schema <https://github.com/lsst/alert_packet/tree/main/python/lsst/alert/packet/schema>`_.


Processing
----------

Alert packets are the result of :doc:`/processing/alert-gen/index`.


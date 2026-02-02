.. _alerts:

#############
Alert packets
#############

Ascii files of measurements for 5-sigma source detections in difference images.

|alerts_doi|


Access
======

Alert packet contents are world public and have no proprietary period (see Section 3 of the `Rubin Data Policy <http://ls.st/rdo-013>`_).

Schema: `Alert Production Database (APDB) <https://sdm-schemas.lsst.io/apdb.html>`_


Brokers
-------

Realtime access to alerts is available through community alert brokers.
Alerts are delivered to :doc:`/access/brokers` and accessed via individual brokers' interfaces.


Alert archive
-------------

.. important::

   The Alert Archive interface is not yet available.

As-transmitted alerts are stored in an archive accessible to Data Rights holders via the Rubin Science Platform.


Description
===========

Each alert packet contains information triggered by a single :doc:`/products/catalogs/dia_source`: a positive or negative detection of at least 5 sigma in a difference image.


Contents
--------

Each alert packet contains:

* unique identifier
* the :doc:`/products/catalogs/dia_source` record that triggered the alert
* the associated :doc:`/products/catalogs/dia_object` or :doc:`/products/catalogs/ss_object` record
* associated :doc:`/products/catalogs/dia_source` and :doc:`/products/catalogs/dia_forced_source` records from the past 12 months
* FITS-formatted cutout postage stamps from the :doc:`/products/images/visit_image`, :doc:`/products/images/difference_image`, :doc:`/products/images/template_coadd`


Schema
------

A detailed description of the records contained within the alert packet is available at `sdm-schemas.lsst.io <https://sdm-schemas.lsst.io/apdb.html>`_.

Alert packets are serialized in the binary Apache Avro format.
The Avro schema is `alert packet schema <https://github.com/lsst/alert_packet/tree/main/python/lsst/alert/packet/schema>`_.


Processing
----------

Alert packets are the result of :doc:`/processing/alert-gen/index`.


.. _catalogs-nearby-sso:

##########
Nearby SSO
##########

.. important::

   Prompt catalogs are not yet available.


One row per nearest predicted known Solar System object for each ``DiaSource``.


Access
======

**The Nearby SSO catalog is not yet accessible.**

TAP
---

* Table name: ``NearbySSO``
* Schema: `PPDB NearbySSO table <https://sdm-schemas.lsst.io/ppdb.html#NearbySSO>`_

Butler
------

* Dataset type: ('nearby_sso', {}, ArrowAstropy)
* Format: Parquet


Description
===========

One row per nearest predicted known Solar System object for each ``DiaSource``, including the nearest objects predicted to land on a detector but match no ``DiaSource`` (with ``DiaSourceId`` = NULL).

Processing
----------

The NearbySSO catalog is the result of :doc:`/processing/moving/index`.

Tutorials
---------

Coming soon.
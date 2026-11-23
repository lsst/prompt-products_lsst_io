.. _catalogs-current-identifications:

#####################################
Current identifications (coming soon)
#####################################

.. important::

   Prompt catalogs are not yet available.


The current identifications catalog produced by the `Minor Planet Center <https://minorplanetcenter.net/>`_ (MPC).

Access
======

**The current identifications catalog is not yet accessible.**


TAP
---

* Table name: ``current_identifications``
* Schema: `PPDB current identifications table <https://sdm-schemas.lsst.io/apdb.html#current_identifications>`_


Description
===========

All solar system objects discovered by Rubin Observatory with LSSTCam are reported to the `Minor Planet Center <https://minorplanetcenter.net/>`_ (MPC).
The MPC has assigned `"observatory code" <https://minorplanetcenter.net/iau/lists/ObsCodesF.html>`_ ``X05`` to the Rubin Observatory.

The MPC is the single worldwide location for receipt and distribution of positional measurements of small bodies in the Solar System.
The MPC is responsible for the identification, designation, and orbit computation for all moving objects.

The ``current_identifications`` table contains an MPC mapping of primary and secondary provisional designations for the same physical object.

Processing
----------

The current identifications catalog is the result of :doc:`/processing/moving/index`.

Tutorials
---------

Coming soon.

.. _catalogs-mpcorb:

########################
MPC orbits (coming soon)
########################

.. important::

   Prompt catalogs are not yet available.


The orbit catalog produced by the `Minor Planet Center <https://minorplanetcenter.net/>`_ (MPC).

Access
======

**The MPC orbits catalog is not yet accessible.**


TAP
---

* Table name: ``mpc_orbits``
* Schema: `APDB MPC orbits table <https://sdm-schemas.lsst.io/apdb.html#mpc_orbits>`_


Description
===========

All solar system objects discovered by Rubin Observatory with LSSTCam are reported to the `Minor Planets Center <https://minorplanetcenter.net/>`_ (MPC).
The MPC has assigned `"observatory code" <https://minorplanetcenter.net/iau/lists/ObsCodesF.html>`_ ``X05`` to the Rubin Observatory.

The MPC is the single worldwide location for receipt and distribution of positional measurements of small bodies in the Solar System.
The MPC is responsible for the identification, designation, and orbit computation for all moving objects.

The ``mpc_orbits`` table contains the orbital parameters calculated by the MPC for all known solar system objects and the linked difference-image detections of moving objects submitted by Rubin (i.e., this catalog is not limited to Rubin discoveries or observations only).

Processing
----------

The MPC orbits catalog is the result of :doc:`/processing/moving/index`.

Tutorials
---------

Coming soon.

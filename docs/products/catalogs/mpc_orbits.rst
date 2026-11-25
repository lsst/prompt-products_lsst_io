.. _catalogs-mpc-orbits:

##########
MPC orbits
##########

.. important::

   The Solar System tables are available via the TAP service as of Nov 30 2026.


The orbit catalog produced by the `Minor Planet Center <https://minorplanetcenter.net/>`_ (MPC).

* Schema: `PPDB MPC orbits table <https://sdm-schemas.lsst.io/ppdb.html#mpc_orbits>`_

Access
======

As of Nov 30 2026, the SS tables are accessible via the TAP service.
In early 2027, these tables will become accessible via the Butler (see the `timeline for early science <https://rubinobservatory.org/for-scientists/resources/early-science>`_).


TAP
---

* |mpc_orbits_doi|
* Table name: ``mpc_orbits``


Description
===========

The `Minor Planet Center <https://minorplanetcenter.net/>`_ (MPC) is the single worldwide location for receipt and distribution of positional measurements of small bodies.
The MPC is responsible for the identification, designation, and orbit computation for all moving objects.

All solar system objects discovered by Rubin Observatory with LSSTCam are reported to the `Minor Planet Center <https://minorplanetcenter.net/>`_ (MPC).
The MPC has assigned `"observatory code" <https://minorplanetcenter.net/iau/lists/ObsCodesF.html>`_ ``X05`` to the Rubin Observatory.

The PPDB ``mpc_orbits`` table is a daily snapshot of the `MPC PostgreSQL orbit table <https://data.minorplanetcenter.net/mpcops/documentation/replicated-tables-schema/>`_.
It contains MPC orbital elements, photometric parameters, fit metadata, and uncertainties for all known Solar System objects, computed by the MPC from the observations of all programs (i.e., it is not limited to objects discovered or observed by Rubin). It contains no individual observations.
Note that the orbit epoch, ``epoch_mjd``, is in Terrestrial Time (TT), not TAI (see :doc:`/products/time_measurements/index`).
The upstream definitions and field semantics are documented in the `MPC replicated-tables schema <https://docs.minorplanetcenter.net/mpc-ops-docs/data-and-services/replicated-tables-schema/>`_.

The ``designation`` column contains the unpacked primary provisional designation.
It is identical row by row to ``unpacked_primary_provisional_designation`` and provides a readable join key to the ``SSObservation`` and ``SSObject`` tables.
For improved performance, SQL joins on integer identifiers such as ``ssObjectId`` are preferred wherever they are available.

The ``mpc_orbits`` table can be joined to the :doc:`SSObject <ss_object>` and :doc:`SSObservation <ss_observation>` tables on the ``designation`` to obtain Rubin-derived object-level photometry and summary properties (``SSObject``) and Minor Planet Center (MPC)-submitted observations (``SSObservation``).

A new snapshot of the ``mpc_orbits`` catalog is taken from the MPC daily, so it includes the newest discoveries and orbit updates from all programs, Rubin's included.


Processing
----------

The MPC orbits catalog is a daily snapshot of the MPC's orbit catalog; it is used by :doc:`/processing/moving/index` but not modified.

Tutorials
---------

See the 200-level catalog :doc:`/tutorials/index` for a notebook on the MPC orbits table.

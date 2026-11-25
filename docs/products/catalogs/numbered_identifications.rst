.. _catalogs-numbered-identifications:

########################
Numbered identifications
########################

.. important::

   The Solar System tables are available via the TAP service as of Nov 30 2026.


The numbered identifications catalog produced by the `Minor Planet Center <https://minorplanetcenter.net/>`_ (MPC).

* Schema: `PPDB Numbered identifications table <https://sdm-schemas.lsst.io/ppdb.html#numbered_identifications>`_

Access
======

As of Nov 30 2026, the SS tables are accessible via the TAP service.
In early 2027, these tables will become accessible via the Butler (see the `timeline for early science <https://rubinobservatory.org/for-scientists/resources/early-science>`_).


TAP
---

* |numbered_identifications_doi|
* Table name: ``numbered_identifications``


Description
===========

The `Minor Planet Center <https://minorplanetcenter.net/>`_ (MPC) is the single worldwide location for receipt and distribution of positional measurements of small bodies.
The MPC is responsible for the identification, designation, and orbit computation for all moving objects.

All solar system objects discovered by Rubin Observatory with LSSTCam are reported to the `Minor Planet Center <https://minorplanetcenter.net/>`_ (MPC).
The MPC has assigned `"observatory code" <https://minorplanetcenter.net/iau/lists/ObsCodesF.html>`_ ``X05`` to the Rubin Observatory.

The PPDB ``numbered_identifications`` table is a daily auxiliary snapshot adopted from the `Minor Planet Center PostgreSQL database <https://data.minorplanetcenter.net/mpcops/documentation/replicated-tables-schema/>`_.
It connects an object's permanent number to its primary provisional designation and includes IAU designations, names, discovery credits, and publication references where available.
The upstream definitions and field semantics are documented in the `MPC replicated-tables schema <https://docs.minorplanetcenter.net/mpc-ops-docs/data-and-services/replicated-tables-schema/>`_.

This table is useful for translating between permanent numbers and the provisional designations used by the Rubin-derived Solar System tables.

The ``numbered_identifications`` table can be joined to the :doc:`mpc_orbits <mpc_orbits>` table on the ``packed_primary_provisional_designation`` to obtain Minor Planet Center (MPC)-derived orbital parameters.

A new snapshot of the ``numbered_identifications`` catalog is taken from the MPC daily, so it reflects the MPC's current designations.


Processing
----------

The numbered identifications catalog is a daily snapshot of the MPC's table; it is used by :doc:`/processing/moving/index` but not modified.

Tutorials
---------

See the 200-level catalog :doc:`/tutorials/index` for a notebook on the Numbered identifications table.

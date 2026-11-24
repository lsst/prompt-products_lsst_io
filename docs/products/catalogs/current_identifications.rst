.. _catalogs-current-identifications:

#######################
Current identifications
#######################

.. important::

   Prompt catalogs are not yet available.


The current identifications catalog produced by the `Minor Planet Center <https://minorplanetcenter.net/>`_ (MPC).

* Schema: `PPDB Current identifications table <https://sdm-schemas.lsst.io/ppdb.html#current_identifications>`_

Access
======

As of Nov 30 2026, the SS tables are accessible via the TAP service.
In early 2027, these tables will become accessible via the Butler (see the `timeline for early science <https://rubinobservatory.org/for-scientists/resources/early-science>`_).


TAP
---

|current_identifications_doi|
* Table name: ``current_identifications``


Description
===========

The `Minor Planet Center <https://minorplanetcenter.net/>`_ (MPC) is the single worldwide location for receipt and distribution of positional measurements of small bodies.
The MPC is responsible for the identification, designation, and orbit computation for all moving objects.

All solar system objects discovered by Rubin Observatory with LSSTCam are reported to the `Minor Planet Center <https://minorplanetcenter.net/>`_ (MPC).
The MPC has assigned `"observatory code" <https://minorplanetcenter.net/iau/lists/ObsCodesF.html>`_ ``X05`` to the Rubin Observatory.

The PPDB ``current_identifications`` table is a daily auxiliary snapshot adopted from the `Minor Planet Center PostgreSQL database <https://data.minorplanetcenter.net/mpcops/documentation/replicated-tables-schema/>`_.
It maps primary and secondary provisional designations that refer to the same physical object and includes both self-mappings and mappings among aliases.
The upstream definitions and field semantics are documented in the `MPC replicated-tables schema <https://docs.minorplanetcenter.net/mpc-ops-docs/data-and-services/replicated-tables-schema/>`_.

This table is useful for resolving historical or alternate designations before joining to the Rubin-derived Solar System tables.

The ``current_identifications`` table can be joined to the :doc:`mpc_ortbis <mpc_orbits>` table on the ``packed_primary_provisional_designation`` to obtain Minor Planet Center (MPC)-derived orbital parameters.

The PPDB contains all alerts from roughly September 2025 onwards as well as the accumulated Solar System tables. The ``current_identifications`` catalog is rebuilt daily from the moving object observations Rubin submits to and are accepted by the MPC as well as discoveries and detections by other contemporaneous programs made during the past 24 hours.

Processing
----------

The current identifications catalog is the result of :doc:`/processing/moving/index`.

Tutorials
---------

See the 200-level catalog :doc:`/tutorials/index` for a notebook on the Current identifications table.

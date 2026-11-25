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

``numbered_identifications`` is a daily copy of the `MPC database <https://data.minorplanetcenter.net/mpcops/documentation/replicated-tables-schema/>`_ table of numbered objects.
Each row gives an object's permanent number (``permid``) and its primary provisional designation, with its IAU designation, name, and naming credit where available.
Use it to translate a number into the primary designation used by the Rubin tables.

The columns keep the MPC's definitions; see the `MPC replicated-tables schema <https://docs.minorplanetcenter.net/mpc-ops-docs/data-and-services/replicated-tables-schema/>`_.
For the MPC's role, see :doc:`mpc_orbits <mpc_orbits>`.

Joins
-----

- ``unpacked_primary_provisional_designation`` to ``designation`` in :doc:`SSObservation <ss_observation>`, :doc:`SSObject <ss_object>`, :doc:`NearbySSO <nearby_sso>`, and :doc:`mpc_orbits <mpc_orbits>`.
- ``packed_primary_provisional_designation`` to :doc:`mpc_orbits <mpc_orbits>`.

Updates
-------

A new copy is taken daily, so it reflects the MPC's current numbering.


Processing
----------

The numbered identifications catalog is a daily snapshot of the MPC's table; it is used by :doc:`/processing/moving/index` but not modified.

Tutorials
---------

See the 200-level catalog :doc:`/tutorials/index` for a notebook on the Numbered identifications table.

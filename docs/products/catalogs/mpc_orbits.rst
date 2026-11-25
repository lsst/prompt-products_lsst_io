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

``mpc_orbits`` is a daily copy of the orbit table of the `MPC database <https://data.minorplanetcenter.net/mpcops/documentation/replicated-tables-schema/>`_.
It has the orbital elements, photometric parameters, uncertainties, and fit information of every known Solar System object.
The MPC computes these from the observations of all observatories, not only Rubin's.
The table contains no individual observations.

The `Minor Planet Center <https://minorplanetcenter.net/>`_ (MPC) receives and distributes the world's positional measurements of small bodies.
It identifies and designates objects and computes their orbits.
Rubin's `observatory code <https://minorplanetcenter.net/iau/lists/ObsCodesF.html>`_ is ``X05``.

Key columns
-----------

- ``designation``: the unpacked primary provisional designation, identical to ``unpacked_primary_provisional_designation``.
  It is added as a join key to the Rubin tables.
- ``epoch_mjd``: the orbit epoch, in Terrestrial Time (TT), not TAI (see :doc:`/products/time_measurements/index`).

The other columns keep the MPC's definitions; see the `MPC replicated-tables schema <https://docs.minorplanetcenter.net/mpc-ops-docs/data-and-services/replicated-tables-schema/>`_.

Joins
-----

- To :doc:`SSObservation <ss_observation>` and :doc:`SSObject <ss_object>` on ``designation``.
- To :doc:`current_identifications <current_identifications>` and :doc:`numbered_identifications <numbered_identifications>` on ``packed_primary_provisional_designation``.

Updates
-------

A new copy is taken daily, with the newest discoveries and orbit updates from all observatories.


Processing
----------

The MPC orbits catalog is a daily snapshot of the MPC's orbit catalog; it is used by :doc:`/processing/moving/index` but not modified.

Tutorials
---------

See the 200-level catalog :doc:`/tutorials/index` for a notebook on the MPC orbits table.

.. _association:

##################
Source Association
##################



DIA object association
======================

``DiaSources`` are associated with known solar system objects and stationary objects using an association radius of 1 arcsecond.

Solar System objects are stored in the ``SSObject`` catalog and stationary objects are stored in the ``DiaObject`` catalog.
``DiaSources`` not associated with either will create new ``DiaObjects``.

Variability characterization
----------------------------

A small set of light curve characterization parameters (timeseries features) are calculated for all ``DiaObjects``.


Alert Production Database
=========================

The live results of association are held in a Cassandra database known as the Alert Production Database (APDB).

Daily exports of the APDB contents will be make available to Data Rights holders for query in the Prompt Products Database (PPDB).

Duplication
-----------
Because is run in real time, if two consecutive images of the same sky area are processed simultaneously, it is possible for two new ``DiaObjects`` to be created at the same position.
These duplicates will be cleaned up in morning processing.
Similarly, alerts may not include the ``DiaSources`` from images taken immediately prior to the current image.
Such ``DiaSources`` will appear in future alerts to that ``DiaObject``, however.
This situation is most common for deep drilling fields but can also occur in field overlaps.
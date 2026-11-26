.. _solar-system-concepts:

###########################
How the data fit together
###########################

This page explains the ideas behind the Solar System tables: what counts as an observation, how one gets into the tables, and how to identify objects.
Read it once; the rest of the guide builds on it.


Observations, objects, and predictions
======================================

The tables make three kinds of statements, and it helps to keep them apart.

**An observation**: "Rubin observed this object, here and at this time."
These are the rows of ``SSObservation``.
Rubin treats a detection as an observation of an object once the MPC has accepted it and identified it with that object.

**An object**: "Here is what Rubin's observations tell us about this object."
These are the rows of ``SSObject``, computed from the object's observations and its orbit in ``mpc_orbits``.

**A prediction**: "According to its orbit, this known object should have been here."
These are the rows of ``NearbySSO``.
A prediction near a detection says the detection *might* be that object.
It is a statement about proximity, not an identification.


From a detection to an observation
==================================

A detection becomes a row of ``SSObservation`` in four steps:

1. Rubin detects a source on a difference image (or, where no template exists yet, on the visit image).
2. Rubin's pipelines associate it with a known object or link it with other detections into a new one, and submit it to the MPC.
3. The MPC accepts it and publishes it.
4. The next daily rebuild of the tables picks it up.

Two things follow.
First, ``SSObservation`` and ``SSObject`` lag the alerts: an observation appears only after the MPC has accepted and published it.
Second, ``SSObservation`` generally doesn't contain single-night detections that can't yet be linked to an object (singletons): Rubin is currently not submitting them to the MPC, at the MPC's request, with some exceptions, mainly for distant objects such as TNOs.


Where the measurements come from
================================

Each ``SSObservation`` row carries the full measurement Rubin submitted to the MPC: position, fluxes, shape, flags, and more.
These measurements come from several kinds of processing:

- the alert production pipeline, which processes each visit as it is taken;
- reprocessings of earlier data, such as Data Preview 1 and Data Preview 2;
- measurements on visit images (Sources), used where no template exists yet.

The ``processing`` column tells you which processing a measurement comes from, and ``measuredOn`` whether it was measured on a difference image (``'difference'``) or on a visit image (``'science'``).
In each case, ``psfFlux`` is the object's flux as measured on that image.

Because the measurements come from several processings, ``SSObservation`` covers a longer period than the alerts in the PPDB, starting with Rubin's earliest observations.


Identifying objects
===================

The tables identify objects by their **primary provisional designation**, in unpacked form, in a column called ``designation`` (for example, ``1930 BM`` for Pluto).
This is the designation the MPC uses as an object's main identifier.

To start from a number or from another designation, translate it first:

- ``numbered_identifications`` maps permanent numbers (``permid``) to primary designations; it includes numbered comets such as ``29P``;
- ``current_identifications`` maps each secondary designation to its object's primary designation.

Objects in the ``SSObject`` table are also identified by an integer ``ssObjectId``, which you can use to join ``SSObject``, ``SSObservation``, and ``NearbySSO``.


The Solar System tables and DiaSource are separate datasets
===========================================================

The PPDB also contains Rubin's alert data: ``DiaSource``, ``DiaObject``, and ``DiaForcedSource``.
Think of these and the Solar System tables as two separate datasets; you should almost never need to join them.
``SSObservation`` already contains the full measurement of every observation, and many of its measurements are not in ``DiaSource`` at all.
(Its ``diaSourceId`` refers to the PPDB's ``DiaSource`` table only in rows with ``processing = 'AP-DS'``.)

The exception is ``NearbySSO``, which is built on the PPDB's ``DiaSources``: join it to ``DiaSource`` on ``diaSourceId`` to get the measurements of the detections it lists.


Time
====

All times in the Solar System tables are in TAI, as Modified Julian Dates.
TAI is currently 37 seconds ahead of UTC, the time scale of MPC observation records and of JPL Horizons' default output.
The one exception is the orbit epoch in ``mpc_orbits``, ``epoch_mjd``, which is in TT.
See :doc:`/products/time_measurements/index` for converting between time scales.

In ``SSObservation``, ``midpointMjdTai`` is the time of the observation at the detector: the middle of the exposure, at the source's position on the focal plane.
Rubin's camera shutter sweeps across the focal plane, so different parts of an image are exposed at slightly different times; ``midpointMjdTai`` is corrected for this.
(``midpointMjdTai_flag`` marks the rare rows where no correction was available and the visit's midpoint is used instead.)


Positions
=========

Measured positions (``ra``, ``dec``) and predicted positions (``ephRa``, ``ephDec``) are both in ICRS, as seen from Rubin.
Offsets in right ascension, such as ``ephOffsetRa``, include the cos(dec) factor.


Where to go next
================

:doc:`tables` takes you through each table and its most useful columns.

.. _catalogs-visit-table:

#####
Visit
#####

.. important::

   **Placeholder.** Prompt catalogs are not yet available.


Observation metadata for the full focal plane (date, time, band, coordinates).

Schema:

Access
======

The visit catalog is accessible via ...

**Recommended access service:**

TAP
---

* doi
* Table name:
* Columns:
* Rows:

Butler
------

* doi
* dataset type
* Format: Parquet
* Number of Butler datasets:


Description
===========

A "visit" is an observation in a single filter, obtained at a given time and sky coordinate.
It refers to an observation with the full focal plane, with the boresight (center) as the
reference for the observational metadata (e.g., airmass).

Tutorials
---------

RBD
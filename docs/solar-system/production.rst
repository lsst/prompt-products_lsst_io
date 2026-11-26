.. _solar-system-production:

##########################
How the tables are made
##########################

You don't need this page to use the tables, but it helps to know where the numbers come from.

.. image:: /processing/moving/images/LSST-Solar-System-Processing-Infographic-PPDB-production.png


The daily cycle
===============

Every night, Rubin observes, processes each visit within minutes, and sends out alerts.
Its Solar System pipelines associate detections with known objects and link new ones, and the observations are submitted to the MPC.
See :doc:`/processing/moving/ss_prompt` for the full cycle, and :doc:`/processing/moving/ss_linking` for how new objects are found.

Once a day, the Solar System tables are rebuilt from scratch:

1. **A snapshot of the MPC database is taken**: the orbits and designations of all known objects, and the MPC's record of Rubin's accepted observations.
2. **Each accepted observation is matched** to the measurement Rubin submitted for it, from whichever processing it came from.
3. **Ephemerides are computed** for every object from its orbit in ``mpc_orbits``, with the ASSIST integrator, at each observation's time. These give the predicted positions, magnitudes, and observing geometry.
4. **Predictions are matched to every PPDB detection**, giving ``NearbySSO``.
5. **Per-object quantities are computed**, giving ``SSObject``.
6. **The tables are checked** for internal consistency, then loaded into the PPDB, replacing the previous day's.


Where to go next
================

Back to :doc:`index`.

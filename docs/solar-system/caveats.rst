.. _solar-system-caveats:

##############
Things to know
##############

The points that most often trip people up, gathered in one place.
Most are explained in more detail elsewhere in this guide.


About the data
==============

**SSObservation and SSObject lag the alerts.**
An observation appears only after the MPC has accepted and published it.

**Singletons are not in the tables.**
Single-night detections that can't yet be linked to an object are currently not being submitted to the MPC, at the MPC's request.

**The tables change every day.**
They follow the MPC's latest observations, orbits, and designations, so the same query can return different results tomorrow.
Save the results you publish.

**NearbySSO shows proximity, not identification.**
A detection near a predicted position may be a different object or an artifact, especially when ``ephOffset`` is large compared with the prediction's uncertainty.
Confirmed observations are in ``SSObservation``.

**Don't store ssObjectId.**
It changes if the MPC changes an object's primary designation.
Record the designation or number instead.

**Check the phase-curve fits.**
Where ``{band}_slope_fit_failed`` is true, G12 was fixed at 0.5 and only H was fitted.
For comets, the H-G12 fit is not meaningful.

**The PPDB is not built for population statistics.**
What reaches the tables depends on detection, linking, and MPC acceptance, and that selection is not characterized here.
Use a data release for population studies.

**Mind the units.**
Positions and position uncertainties are in degrees, but ``ephOffset`` and its components are in arcseconds.
Fluxes are in nJy.
Times are TAI, except ``mpc_orbits.epoch_mjd``, which is TT.


About the TAP service
=====================

The PPDB's TAP service doesn't yet support all of ADQL.
At present:

- **Boolean columns can't be used in conditions.**
  ``WHERE psfFlux_flag = 0`` (or ``= FALSE``, ``IS FALSE``, ``NOT psfFlux_flag``) fails.
  Retrieve the flags and filter afterwards, as in :doc:`recipes`.
- **Some functions are missing:** ``RADIANS``, ``DEGREES``, and ``PI`` fail.
  Convert angles yourself; to get radians from degrees, multiply by 0.0174532925.
  ``SIN``, ``COS``, ``SQRT``, ``LOG10``, and ``POWER`` work.
- **Run large queries one at a time.**
  Several large asynchronous queries running at once can hang.



Getting help
============

To ask a question or report a problem, post in the `Support category of the Rubin Community Forum <https://www.rubin.community/c/support/6>`_.
For how to cite the data, see :doc:`/cite`; for data rights and access, see :doc:`/access/index`.

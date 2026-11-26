.. _solar-system-caveats:

##############
Things to know
##############

The points that most often trip people up, gathered in one place.
Most are explained in more detail elsewhere in this guide.


About the data
==============

**SSObservation and SSObject lag the alerts, and generally don't contain singletons.**
An observation appears only after the MPC has accepted and published it; singletons are currently not being submitted to the MPC, at the MPC's request, with some exceptions, mainly for TNOs.

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
Where ``{band}_slope_fit_failed`` is true, G12 was fixed at 0.5; for comets, the fit is not meaningful.

**The PPDB is not built for population statistics.**
Its selection is not characterized; use a data release for population studies.

**Mind the units.**
Positions and position uncertainties are in degrees, but ``ephOffset`` and its components are in arcseconds.
Fluxes are in nJy; distances in au.
Times are TAI (UTC + 37 s), except ``mpc_orbits.epoch_mjd``, which is TT, and are not corrected for light travel time.


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
  Use asynchronous queries for large results, and don't run several at once: they can hang.



Getting help
============

To ask a question or report a problem, post in the `Support category of the Rubin Community Forum <https://www.rubin.community/c/support/6>`_.
For how to cite the data, see :doc:`/cite`; for data rights and access, see :doc:`/access/index`.

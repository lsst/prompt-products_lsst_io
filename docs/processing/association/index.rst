.. _association:

##################
Source Association
##################



DIA object association
======================

``DiaSources`` are associated with known solar system objects and stationary objects using an association radius of 1 arcsecond.

Solar System objects are stored in the ``SSObject`` catalog and stationary objects are stored in the ``DiaObject`` catalog.
``DiaSources`` not associated with either will create new ``DiaObjects``. ``DiaSources`` associated with stationary objects will contain a non-null ``diaObjectId``, while ``DiaObjects`` associated with solar system objects will include a non-null ``ssObjectId``.
Each associated ``DiaSource`` will either have a non-null ``diaObjectId`` or ``ssObjectId`` but will not have both fields populated.

The time (in MJD, TAI) when a ``DiaSource`` was reassociated from a ``DiaObject`` to ``SSObject`` (if such reassociation happens) will be included in the ``DiaSource`` catalog.

Solar System object association
-------------------------------

Alerts for ``DiaSources`` with known solar system objects predicted to be within the 1-arcsecond association radius of the ``DiaSource`` detection include the ``ssSource`` record.
The ``ssSource`` table contains information derived from the known orbit of the associated solar system object and the current observation of the ``DiaSource``.

Because a single known moving object predicted position may fall within the 1-arcsecond association radius for multiple nearby ``DiaSources``, a ``diaDistanceRank`` field is included in the ``ssSource`` table with the rank of the ``diaSourceId`` -identified source in terms of its closeness to the predicted solar system object (SSO) position.
If the ``diaSourceId`` is the nearest ``DiaSource`` to this SSO prediction, ``diaDistanceRank`` = 1 would be set.
If it is the second nearest, it would be 2, etc.
In this framework, multiple ``DiaSources`` may include the ``ssSource`` record for the same known solar system object, but each would have a different value assigned for the ``diaDistanceRank`` according to their proximity to the predicted posiiton of that solar system object.
To find the best association match between a given ``diaSourceId`` to the predicted position of a known solar system object, filter on ``diaDistanceRank`` == 1 in the ``SSSource`` table.
In addition, because ``SSSources`` (detections) are attributed to ``SSObjects``, a given ``ssObjectId`` may have ``SSSources`` associated to that ``ssObjectId`` that include different ``diaDistanceRank`` values.
Similarly in this case, to find the best association match between a given ``ssObjectId`` to the predicted position of a known solar system object, filter on ``diaDistanceRank`` == 1 in the ``SSSource`` table.

Also included in the ``ssSource`` catalog is the ephemeris offset (``ephOffset``) that measures the total observed versus predicted angular separation on the sky between the observed ``DiaSource`` and the predicted position of a known solar system object.
This field is also useful for quality control, association confidence, and filtering uncertain matches.
The along-track and cross-track ephemeris offsets (``ephOffsetAlongTrack`` and ``ephOffsetCrossTrack``, respectively) as well as the RA and Dec ephemeris offsets (``ephOffsetRa`` and ``ephOffsetDec``, respectively) between the observed ``DiaSource`` and the predicted position of a known solar system object are also included in the ``ssSource`` catalog.

To decipher ambiguous associations, the ``diaDistanceRank``, ephemeris offsets, and multiple associated detections can be used.
Orbit-fitting, performed by the IAU Minor Planet Center (MPC), after ``DiaSource`` detections associated with known solar system objects in the above ranked system are submitted to the MPC and subsequently ingested by the Solar System Processing pipelines in the ``mpc_orbits`` catalog, will provide the most concrete solar system object association.

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
.. _filtering:

#################################
Filtering and Reliability Scoring
#################################

To reduce the proportion of spurious alerts sent to users, several layers of filtering are applied.
These will continue to be tuned over time as we obtain more on-sky data.


Filtering
=========

We filter artifacts at several stages.

At the difference image detection stage, we reject sources withe pixels off image; edge; interpolated bad, edge, no data, saturated, and saturated template center pixels; as well as those occuring in modeled diffraction spikes.

We later filter sources with cosmic rays in their center pixels as well as those with high template variance and those with negative direct image fluxes indicative of background subtraction problems.


Artificial Satellites
=====================

We aim to remove artifical satellites from the alert stream.
We use predicted positions of known satellites to filter ``DiaSources`` prior to alerts.
Trailed sources with motions faster than 10 deg/day are also filtered.

"Glint trails" due to rotating space junks are identified by linear fits to the detected ``DiaSources``.
Glints are not filtered, but are flagged with a ``glint_trail`` boolean.


Reliability
===========

A Convolutional Neural Network (CNN) was trained with a mixture of simulated data and human-labeled transients, variables, and moving objects.
It uses cutout science, template, and difference images to estimate whether a ``DiaSource`` is likely astrophysical.
The resulting ``reliability`` score ranges from 0-1, with larger scores corresponding to more likely real objects.
To minimize the number of false positives, we presently filter ``DiaSources`` with reliability scores less than 0.1.
Users may select alerts with higher reliability scores to maximize purity.


.. important::

   Detailed performance information about the reliability model as well as suggested thresholds are forthcoming.


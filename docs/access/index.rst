###########
Data access
###########

Services, tools, and policies for accessing the alerts and the Prompt data products.

Data policy
===========

**Public:** The contents of the alert packets and the Prompt Products Database (PPDB), plus all data submitted to the MPC, are public.
Anyone, anywhere, worldwide, may share, analyze, and publish papers based on public data.
Alerts are publicly accessible via the brokers.

**Proprietary:** The promptly-processed images and the direct-image source catalogs are proprietary, and are subject to an initial 80-hour embargo.
Only Rubin data rights holders may access and analyze proprietary data after the embargo period.
Access to the Rubin Science Platform (RSP) is also proprietary.

**Data rights:** All scientists and students in the US and Chile, plus named members of international in-kind teams, are Rubin data rights holders.
`Learn more about the Rubin data policy <https://rubinobservatory.org/for-scientists/data-products/data-policy>`_.


Alert brokers
=============

**Alert broker:** Software systems that ingest and process astronomical alerts from the LSST and other surveys, and serve them to the scientific community.

**Alert filter:** A set of rules that an alert packet either passes or fails.
E.g., "if brighter than 21st magnitude, and if discovered less than 6 days ago, and if two detections exist, then pass" would be is a very simple filter.

Find more information about the nine alert brokers and how to access them on the `Alerts and brokers for scientists webpage <https://rubinobservatory.org/for-scientists/data-products/alerts-and-brokers>`_.


Community filters
-----------------

Up to 20 community alert filters will be defined, implemented, validated, and maintained by Rubin staff with input from the broad Rubin science community, and with support from the ANTARES broker.

These community filters will be designed to serve a variety of common time-domain science goals, and lower the barrier to entry into alert-based science.

See the "Roadmap for Community Alert Filters with the ANTARES Broker" `RTN-090 <https://rtn-090.lsst.io/>`_.



Minor Planets Center (MPC)
==========================

All solar system objects discovered by Rubin Observatory with LSSTCam are reported to and available via the `Minor Planets Center <https://minorplanetcenter.net/>`_.

**B612**: A subset of MPC entries that include only Rubin observations (`"observatory code" <https://minorplanetcenter.net/iau/lists/ObsCodesF.html>`_ ``X05``) is maintained by the Asteriod Institute, a program of the `B612 Foundation <https://b612foundation.org/>`_, and available at `ls.st/ast <http://ls.st/ast>`_.

|b612_mpc_x05|



Rubin Science Platform (RSP)
============================

.. important::

   The alert archive, PPDB, and prompt images are not yet available.


Past alerts and Prompt data products will be accessable via the RSP, as demonstrated in the :doc:`/tutorials/index`.

Alert archive
-------------

An archive of all past alerts will be available via the Rubin Science Platform.
This archive will be queryable by the ``DIA Object`` identifier.

Prompt Products Database (PPDB)
-------------------------------

The PPDB will be updated within 24 hours of alert distribution.
The same data that is streamed in alert packets is available for query and analysis via the RSP.

Prompt images
-------------

The promptly-processed visit and difference images will be available after an 80 hour embargo period via the RSP.

Get started
-----------

Follow these `instructions to sign up for an RSP account <https://rsp.lsst.io/guides/getting-started/get-an-account.html>`_, then work through the :doc:`/tutorials/index`.

.. _access-getsupport:

Get support
-----------

To ask question and get help with any aspect of the Rubin data products, services, and tools, please post a new topic in the Rubin Community Forum's `Support category <https://community.lsst.org/c/support/6>`_.

###########
Data access
###########

Services, tools, and policies for accessing the alerts and the Prompt data products.

Data policy
===========

**Data rights:** All scientists and students in the US and Chile, plus named members of international in-kind teams, are Rubin data rights holders who can access proprietary Rubin data products and services.
`Learn more about the Rubin data policy <https://rubinobservatory.org/for-scientists/data-products/data-policy>`_.

**Public:** The contents of the alert packets, the Prompt Products Database (PPDB), and all MPC submissions are public, which means they can be used and shared by anyone, including those without data rights.
Alerts are publicly accessible via the brokers, and the MPC is openly accessible, but the PPDB is accessible only via the Rubin Science Platform (RSP), which is a proprietary service.

**Proprietary:** The Prompt images and the visit image source catalogs will become available via the RSP to Rubin data rights holders after an 80-hour embargo, after which they have a 2-year proprietary period during which they cannot be shared with anyone without data rights.



Rubin Science Platform (RSP)
============================

**Get started:** Follow these `instructions to sign up for an RSP account <https://rsp.lsst.io/guides/getting-started/get-an-account.html>`_.
Refer to the `RSP user guides <https://rsp.lsst.io/>`_ and work through the :doc:`/tutorials/index`.

**Get support:** To ask question and get help with any aspect of the Rubin data products, services, and tools, please post a new topic in the publicly-accessible Rubin Community Forum `Support category <https://www.rubin.community/c/support/6>`_.


Alert archive
-------------

An archive of past :doc:`/products/alerts/index` is available via the RSP.
Alerts can be queried individually using the ``alertId`` (which is the same as the ``diaSourceId``).
In the Notebook Aspect, individual alert packets can be retrieved via the Alert Retrieval Service, and their contents analyzed and visualized programmatically.
In the Portal Aspect, individual alert packets can be loaded into the Alert Viewer and explored via the user interface.
See the :doc:`/tutorials/index` for demonstrations of how to use these tools to access the alert archive.


Prompt Products Database (PPDB)
-------------------------------

The PPDB will be updated within 24 hours of alert distribution.
The same data that is streamed in alert packets is available for query and analysis in the PPDB via the RSP.

Prompt images
-------------

The promptly-processed visit and difference images will be available after an 80-hour embargo period via the RSP.
Measurements for sources detected in the prompt visit images will be also available after an 80 hour embargo period via the RSP.


Alert brokers
=============

**Alert broker:** Software systems that ingest and process astronomical alerts from the LSST and other surveys, and serve them to the scientific community.
Brokers are the primary and optimal interface for real-time science analyses of LSST alerts.

**Alert filter:** A set of rules that an alert packet either passes or fails.
E.g., "if brighter than 21st magnitude, and if discovered less than 6 days ago, and if two detections exist, then pass" would be is a very simple filter.

Find more information about the nine alert brokers and how to access them on the `Alerts and brokers for scientists webpage <https://rubinobservatory.org/for-scientists/data-products/alerts-and-brokers>`_.


Community filters
-----------------

Up to 20 community alert filters will be defined, implemented, validated, and maintained by Rubin staff with input from the broad Rubin science community, and with support from the ANTARES broker.
These community filters will be designed to serve a variety of common time-domain science goals, and lower the barrier to entry into alert-based science.
See the "Roadmap for Community Alert Filters with the ANTARES Broker" `RTN-090 <https://rtn-090.lsst.io/>`_.


Minor Planet Center (MPC)
=========================

All solar system objects discovered by Rubin Observatory with LSSTCam are reported to and available via the `Minor Planet Center <https://minorplanetcenter.net/>`_. A live submissions dashboard is maintained at `sbx.dirac.dev/station/X05 <https://sbx.dirac.dev/station/X05>`_.

**B612**: A subset of MPC entries that include only Rubin observations (`"observatory code" <https://minorplanetcenter.net/iau/lists/ObsCodesF.html>`_ ``X05``) is maintained by the Asteriod Institute, a program of the `B612 Foundation <https://b612foundation.org/>`_, and available at `ls.st/ast <http://ls.st/ast>`_.

|b612_mpc_x05|



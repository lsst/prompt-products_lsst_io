.. _api-101-1:

###################################################
101.1. Remote access to the Alert Retrieval Service
###################################################


For the API Aspect of the Rubin Science Platform (RSP) at data.lsst.cloud.

**Data Release:** Prompt Products

**Last verified to run:** 2026-07-13

**Learning objective:** Retrieve a single alert packet.

**LSST data products:** Alert packets.

**Credit:** Originally developed by the Rubin Community Science team.
Please consider acknowledging them if this tutorial is used for the preparation of journal articles, software releases, or other tutorials.

**Get Support:** Everyone is encouraged to ask questions or raise issues in the `Support Category <https://community.lsst.org/c/support/6>`_ of the Rubin Community Forum.
Rubin staff will respond to all questions posted there.


Introduction
============

This tutorial demonstrates how to remotely retrieve a single LSST alert packet from an external python environment using the API Aspect of the RSP.
It was tested and confirmed to work with the `NOIRLab Astro DataLab <https://datalab.noirlab.edu/data-explorer>`_.


Steps
=====

**1. Create a token.**

Follow the instructions in the RSP user guide for `Creating user tokens <https://rsp.lsst.io/guides/auth/creating-user-tokens.html>`_.
The token must have a scope of ``read:image`` to access the Alert Retrieval Service.

**2. Get the API endpoint.**

All URL for data access via the RSP are available at `data.lsst.cloud/api-aspect <https://data.lsst.cloud/api-aspect>`_.
The Alert Retrieval Service URL is listed under "Prompt".

**3. Install python packages, if needed.**

This tutorial requires the ``requests``, ``fastavro``, and ``io`` python packages.
The first two can be pip-installed and the latter is standard.
These steps have been confirmed to work with, at minimum, python version 3.10, ``requests`` version 2.32.4, and ``fastavro`` version 1.12.0.

**4. Python code for alert retrieval.**

.. code-block:: python

   import requests
   import fastavro
   import io

   token = "<paste-your-token-here>"
   url = "https://data.lsst.cloud/api/alerts"
   alert_id = 170059317401616524
   session = requests.Session()
   session.headers["Authorization"] = f"Bearer {token}"
   response = session.get(url, params={"ID": alert_id})
   packet = fastavro.reader(io.BytesIO(response.content))

**5. Interact with the alert contents.**

From here, use the same code as in :doc:`/tutorials/notebook-prompt/notebook-101-1`.

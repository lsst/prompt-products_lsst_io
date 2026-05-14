.. _portal-101-1:

############################################
101.1. Navigate the Alerts Viewer interface
############################################

For the Portal Aspect of the Rubin Science Platform (RSP) at data.lsst.cloud.

**Data Release:** Prompt Products

**Last verified to run:** 05/13/2026

**Learning objective:** Use the Alert Viewer to access the contents of a single alert packet retrieved by alert ID.

**LSST data products:** Alert packets.

**Credit:** Originally developed by the Rubin Community Science team.
Please consider acknowledging them if this tutorial is used for the preparation of journal articles, software releases, or other tutorials.

**Get Support:** Everyone is encouraged to ask questions or raise issues in the `Support Category <https://community.lsst.org/c/support/6>`_ of the Rubin Community Forum.
Rubin staff will respond to all questions posted there.

----

**1. Log into the RSP and enter the Portal Aspect.**
In a web browser go to `data.lsst.cloud <https://data.lsst.cloud/>`_, log in and click on the "Portal" panel.

**2. Select the Alert Viewer.**
Click on the tab labeled "Alert Viewer". The Alert Viewer requires a known Alert ID. For the purpose of this tutorial, use the Alert ID 170059317401616524. The ALeRCE broker identified the ``DiaObject`` associated with this `alert <https://lsst.alerce.online/object/170059286935240883?survey=lsst>`_ as a potential Active Galactic Nucleus (AGN).

.. figure:: images/portal-101-1-1.png
    :name: portal-101-1-1
    :alt: The landing page for the Alert Viewer, showing the interface to enter an Alert ID and a few example Alert IDs.

    Figure 1: The Alert Viewer search interface.

**3. Retrieve the contents of a single alert packet.**
Enter alert id 170059317401616524 and hit "load". The pencil button next to the "load" button shows Alert IDs from recent searches.

**4. Examine the results for the alert packet.**
The results include the full alert packet which contains a table with records of the triggering ``DiaSource`` (top left panel), a table with associated ``DiaSource`` records (top right panel), a chart for the light curve (top right panel), and cutout postage stamps (bottom panel).

.. figure:: images/portal-101-1-2.png
    :name: portal-101-1-2
    :alt: The default view of the Alert Viewer results showing 3 panels with 2 tables and 3 cutout image stamps.

    Figure 2: The Alert Viewer results interface showing the contents of the packet for Alert ID 170059317401616524.

**4.1. Examine the triggering DiaSource records:**
The results table includes triggering ``DiaSource`` records, scheduler metadata, and associated ``DiaObject`` or ``SSObject``. If the alert is associated with a ``SSObject``, then this table includes records from ``mpc_orbits`` as shown in Figure 3.

.. figure:: images/portal-101-1-3.png
    :name: portal-101-1-3
    :alt: A table showing records for an alert for a Solar System object including columns from the ``mpc_orbits`` table.

    Figure 3: The results table for Alert ID 170059294376985743, associated with a Solar System object.

**4.2. Examine the DiaSource records.**
The results table includes records from the ``DiaSource`` table for the past 12 months for the ``DiaObject`` associated with the alert.

**4.3. Examine the cutout stamps.**
The cutout stamps include a science image cutout, a template image cutout, and a difference image cutout.

**4.4. Examine the light curve.**
Click on 'Chart' to change the view to the light curve for the ``DiaObject`` associated with the alert. The default chart shows the light curve with psfFlux vs midpointMjdTai for all bands.

.. figure:: images/portal-101-1-4.png
    :name: portal-101-1-4
    :alt: The Alert Viewer results interface showing 3 panels with 1 table, a chart with a light curve, and 3 cutout image stamps.

    Figure 4: The results interface showing the chart with the light curve for Alert ID 170059317401616524, a potential AGN.



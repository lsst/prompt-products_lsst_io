.. _portal-101-1:

#######################################
101.1. Use the Alerts Viewer interface
#######################################

For the Portal Aspect of the Rubin Science Platform (RSP) at data.lsst.cloud.

**Data Release:** Prompt Products

**Last verified to run:** 05/08/2026

**Learning objective:** Use the Alert Viewer to access the contents of a single alert packet retrieved by alert ID. 

**LSST data products:** Alert packets.

**Credit:** Originally developed by the Rubin Community Science team.
Please consider acknowledging them if this tutorial is used for the preparation of journal articles, software releases, or other tutorials.

**Get Support:** Everyone is encouraged to ask questions or raise issues in the `Support Category <https://community.lsst.org/c/support/6>`_ of the Rubin Community Forum.
Rubin staff will respond to all questions posted there.

----

**1. Log into the RSP and enter the Portal Aspect.**
In a web browser go to `data.lsst.cloud <https://data.lsst.cloud/>`_, log in and click on the "Portal" panel. 

**2. Select the alert viewer.** 
Click on the tab labeled "Alert Viewer". The Alert Viewer requires a known Alert ID. For the purpose of this tutorial, use the Alert ID 170059317401616524. The ALeRCE broker identified the `object associated with this alert<https://lsst.alerce.online/object/170059286935240883?survey=lsst>` as a potential Active Galactic Nucleus (AGN).   

.. figure:: images/portal-101-1-1.png
    :name: portal-101-1-1
    :alt: 

    Figure 1: The Alert Viewer search interface.

**3. Retrieve the contents of a single alert.**
Enter alert id 170059317401616524 and hit "load". The pencil button next to the "load" button shows Alert IDs from recent searches. 

**4. Examine the results for the alert.** 
The results include the full alert packet, image stamps, contents from the diaSource table, and a light curve. The top left panel includes shows the full alert packet. The top right panel shows the results from the diaSource table for the diaObjectId associated with the alert. The bottom panel shows 3 image staps: the science image, the template image, and the difference image.  

.. figure:: images/portal-101-1-2.png
    :name: portal-101-1-2
    :alt: 

    Figure 2: The default view for the results interface for the Alert Viewer.  

**5. View the light curve.**
Click on 'Chart' to change the view to the light curve for the diaObject associated with the alert. 

.. figure:: images/portal-101-1-3.png
    :name: portal-101-1-3
    :alt: 

    Figure 3: The light curve for the diaObject associated with the alert. 



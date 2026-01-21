.. _citedp1:

###########
How to cite
###########

For general information, see `How to cite Rubin Observatory <https://rubinobservatory.org/for-scientists/documentation/cite>`_ and `citations.lsst.io <https://citations.lsst.io/>`_.


DOI and publications
====================

Data products
-------------

When citing the Prompt Products please reference this paper:

|pp_paper|


When citing the Prompt Products dataset as a whole please use:

|prompt_doi|


If you only use a specific data product (e.g., alerts, or a specific catalog or image type), you may cite the DOI explicitly assigned to that subset.
These DOI will appear on the individual pages under :doc:`/products/index`.


Science pipelines software
--------------------------

The `Rubin Science Pipelines <https://pipelines.lsst.io/>`_ can be formally cited as:

|osprae_doi| [:download:`BibTeX </bib/osprae.bib>`]


Telescope and instrument
------------------------

For AAS publications please refer to the `facility <https://journals.aas.org/facility-keywords/>`_ as **"Rubin:Simonyi"**.

Note that the `Minor Planet Center <https://minorplanetcenter.net/iau/lists/ObsCodesF.html>`_ has allocated the telescope code X05.

The `LSST Camera <https://lsstcam.lsst.io/>`_ should be citing using:

|lsstcam_doi| [:download:`BibTeX </bib/lsstcam.bib>`]


Rubin Science Platform
----------------------

If you accessed data via the RSP, for AAS publications please also add the facility **"Rubin:USDAC"**.

Tutorials
---------

When citing the tutorials please use:

|tutorials_doi| [:download:`BibTeX </bib/tutorials.bib>`]


How to refer to single objects
==============================

If you are referring to individual sources or objects from the Prompt alerts or catalogs, please use the naming convention described here, which has been registered with the International Astronomical Union (IAU) and was developed following `IAU specifications <https://cdsweb.u-strasbg.fr/Dic/iau-spec.html>`_.
All designations should begin with "LSST-P" (denoting the Legacy Survey of Space and Time, Prompt products), followed by a string that specifies which table the object was obtained from.
These strings should be "DO" (``DiaObject``), "DS" (``DiaSource``), or "SSO" (``SSObject``).
Following this table identifier, the (18-digit for all tables except ``SSObject``, for which the ID is 17 digits long) unique numeric identifier from the specified table (i.e., the ``diaObjectId``, ``diaSourceId``, or ``ssObjectId``) should be included.
The "fields" of the identifier should be separated by dashes, so that the designation appears like "LSST-P-TAB-123456789012345678."
To summarize, here are examples for objects from each table:

* ``DiaObject``: LSST-P-DO-609788942606140532 (for ``diaObjectId`` 609788942606140532)
* ``DiaSource``: LSST-P-DS-600359758253260853 (for ``diaSourceId`` 600359758253260853)
* ``SSObject``: LSST-P-SSO-21163611375481943 (for ``ssObjectId`` 21163611375481943)

All catalog entries reported in Prompt tables will have at least one of these three types of IDs.

Reporting data to external systems
==================================

When reporting discoveries or followup information from Prompt products to external archives and messaging mechanisms
(e.g., the IAU Transient Name Service (``https://www.wis-tns.org/``),
please remember to use "LSSTCam" as the instrument name and "Rubin:Simonyi"
(or just "Rubin" if that's not available) as the observatory/facility/telescope name,
and please look for an option to report filters as "LSST" and not "Sloan", despite the similarity in photometric systems.

If you encounter a service where these metadata options are not yet available,
we ask that you bring this to our attention;
a post on the "Support" category on https://community.lsst.org/ would be very much appreciated.

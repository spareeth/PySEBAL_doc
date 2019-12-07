PySEBAL data requirements
*************************

.. toctree::
   :maxdepth: 2
   :caption: Contents

To run PySEBAL we need the following input data as shown below in the figure.

.. figure:: img/data1.png
   :align: center
   :width: 300

Satellite data
==============

Currently PySEBAL support data from Landsat 4/5/7/8, MODIS, PROBA-V/VIIRS sensors/satellites.
In this documentation currently PySEBAL using Landsat data as input is explained. But data from other satellites can be easily used by replacing the Landsat data,

Acquiring Landsat data
++++++++++++++++++++++
The main archive of Landsat satellite (all missions – TM/ETM/OLI) is earth explorer website: https://earthexplorer.usgs.gov/. 

.. note::

   First step is to create user login for this website so that you are able to download the data.


Let us now search and download data for Miandoab irrigation scheme in Iran for the time period April to September 2018.

| **Step1**

| In the “Search Criteria” tab type in your “address/place” of your interest, in this case “Miandoab” and click “enter” or search for Path/row - 168/034.

.. figure:: img/ls1.png
   :align: center
   :width: 300

Click on the result (red box) and a location popup will appear over on the map.

| **Step2**

| Now enter the date range for which data is required in the same tab.

.. figure:: img/ls2.png
   :align: center
   :width: 300

| **Step3**

| Select the datasets you want to search and set the additional criteria of those data with cloud cover less than 20%

.. figure:: img/ls3.png
   :align: center
   :width: 300

Here we are selecting only Landsat 8 data. For previous years you can also try with Landsat 7 ETM, and Landsat 4/5 TM. Finally click the “Results” to see list of all available data with our conditions met for the study area in the given period of time.

| **Step4**

| Check the listed scenes using browse images, select and download the 168/034 scene

.. figure:: img/ls4.png
   :align: center
   :width: 300

The image icon under each result can be used to see the preview of that landsat scene (see Figure above). The download icon can be used to download that single scene with out ordering. While the Bulk download icon can be used to get a single link to download multiple products at a time. More details on bulk download can be found here - https://www.usgs.gov/media/videos/eros-earthexplorer-how-do-a-bulk-download.

.. figure:: img/ls6.png
   :align: center
   :width: 300

For example, image dated 22 June 2018 looks really good, click on the download icon to get the data. You have to login in order to download the data.

.. figure:: img/ls5.png
   :align: center
   :width: 300

From the list of options download the “Level-1 GeoTIFF Data Product” to get all the spectral bands and metadata of the scene.

Bulk download Landsat data using command line
+++++++++++++++++++++++++++++++++++++++++++++

This section explains how you can download big amount of Landsat data from Google cloud bucket using command line.

| **Steps**

* Install gsutil library - https://pypi.org/project/gsutil/
* If you want to list all the Landsat 8 data over Miandoab covering tile 168/034 in June 2018 use the following command:

.. code-block:: Shell
   :linenos:

   gsutil ls -d gs://gcp-public-data-landsat/LC08/01/168/034/LC08_L1TP_168034_201806*T1

* To download them, use the following command:

.. code-block:: Shell
   :linenos:
   
   # '.' means it will download to present directory
   gsutil -m cp -r gs://gcp-public-data-landsat/LC08/01/168/034/LC08_L1TP_168034_201806*T1 .

This `link <https://shuzhanfan.github.io/2018/05/download-landsat8-data-from-google-cloud/>`_ has more details on this approach.

.. Meteo data
.. ==========

.. Meteo data is either obtained from field stations or from global models like GLDAS, ERA5, MERRA2 etc. 


.. Soil hydraulic properties data
.. ==============================

.. Digital Elevation Model
.. =======================

.. * CPU with 2 cores and > 2GHz processor
.. * Minimum of 8 GB RAM
.. * Storage space of 10 - 20 GB if processing multiple landsat tiles


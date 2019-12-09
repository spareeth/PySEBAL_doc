PySEBAL input data arrangement
******************************

.. toctree::
   :maxdepth: 2
   :caption: Contents:

Let us now arrange all the data we prepared to Run PySEBAL and prepare a input excel sheet.

Assuming that you have following folder structure (recommended) for input data:

.. figure:: img/folder.png
   :align: center
   :width: 400
   
   Folder structure of input data

Open the excel sheet provided to you - ``InputEXCEL_v3_3_7_WIN.xlsx``, you can also download it `here <http://scientificpubs.com/misc/InputEXCEL_v3_3_7_WIN.xlsx>`_
Make necessary changes to the excel sheet as listed below.

**Sheet 1: General Input**

* **InputMap** - set the path to your satellite data
* **OutputMap** - Where you want to save PySEBAL outputs (if not existing, PySEBAL will create the folder)
* **Image_type** - satellite type
* **NameDEM** - set the path to DEM file (Note that you need to provide the file name with extension(.tif))

**Sheet 2: Meteo_Input**

* **Temp_inst** - set the path to instantaneous air temperature (including file name and extension)
* **Temp_24** - set the path to daily average air temperature (including file name and extension)
* **RH_inst** - set the path to instantaneous Relative humidity (including file name and extension)
* **RH_24** - set the path to daily average Relative humidity (including file name and extension)
* **Wind_inst** - set the path to instantaneous wind speed (including file name and extension)
* **Wind_24** - set the path to daily average wind speed (including file name and extension)
* **Rs_inst** - set the path to instantaneous downward shortwave radiation (including file name and extension)
* **Rs_24** - set the path to daily downward shortwave radiation (including file name and extension)

**Sheet 3: Soil_Input**

* **Saturated soil moisture content** - set the path with filename and extension
* **Saturated soil moisture content subsoil** - set the path with filename and extension
* **Residual soil moisture content** - set the path with filename and extension
* **Residual soil moisture content subsoil** - set the path with filename and extension
* **Field_Capacity** - set the path with filename and extension
* **Wilting point** - set the path with filename and extension

**Sheet 4: Landsat_Input**

* **Name Landsat Image** - Name of the landsat image bands (for example without ``_B1.TIF``)
* **Landsat Number** - 4/5/7/8 depending which landsat
* **Bands Thermal** - 1/2, In case of Landsat 8, it is 2
* **tscold_min** - Min temperature value to detect cold pixels from DEM corrected surface temperature
* **tscold_max** - Max temperature value to detect cold pixels from DEM corrected surface temperature
* **ndvihot_low** - Min NDVI value to pick the hot pixel
* **ndvihot_high** - Max NDVI value to pick the hot pixel
* **temp_lapse_rate** - Temperature lapse rate for correction of surface temperature


.. note::

   Number of Rows in the input excel sheet is equal to the number of landsat images you want to process. If you have 10 images, the row numbers are from 2 to 11.


Once the input excel sheet is ready, open the Run SEBAL python file which is in ``D:\PySEBAL_dev\SEBAL`` folder.
* If you are running in python 2 - the file is ``Run_py2.py``
* If you are running in python 3 - the file is ``Run_py3.py``

Assuming that you are using python 2, open ``Run_py2.py`` in Notepadd++.

**Edits in Run_py2 file**

We need to make following changes in this file:

* **Line 15** - Set the path to prepared excel sheet
* **Line 16** - There are two modes CALIB/RUN. First run **CALIB** and then after setting the thresholds run in **RUN** mode.
* **Line 17/18** -Set start and end row numbers for running all the landsat images in one go.

Once you made the changes save and close the file ``Run_py2``

Now open **new** **OSGeo4W Shell** and cd to ``PySEBAL_dev\SEBAL`` folder and run the following command.

.. code-block:: python
   :linenos:

   python Run_py2.py

**CALIB/RUN mode - setting the thresholds**

In the **Sheet 4: Landsat_Input** of the input excel sheet we have to set NDVI and Temperature min and max thresholds for cold and hot pixels. If you are fine with the default values, you can go ahead with the **RUN** mode. But if you want to set the thresholds for Landsat data separately, then running first in **CALIB** mode is ideal.

.. note::

   As a rule of thumb, we can use 5th and 10th percentile from corrected surface temperature as low and high cold pixel thresholds. We use 2nd and 5th percentile from NDVI as low and high hot pixel thresholds.

Once PySEBAL finish in "CALIB" mode we can compute the thresholds using the following commands.
Open GRASS GIS in UTM location, in this case UTM38N, create a new location with ``study_area.shp`` file as the georeferenced file. Make a new mapset if you don't have one. This mapset in UTM38N location can be used for further analysis of ETa outputs. 

.. code-block:: python
   :linenos:

   # Open grass gis
   grass78 --gui
   # Open bash
   bash
   # check the region
   g.region -p
   # set the resolution of the region to 30m
   g.region res=30 -a
   # Now change directory to PySEBAL output directory set in excel sheet
   cd /d/FAO_Training_Dec2019/PySEBAL_output/LC08_L1TP_168034_20180622_20180703_01_T1
   # Import the corrected surface temperature to this mapset
   r.import.py Output_thermal/L8_ts_dem_30m_2018_06_06_157.tif output=ts_20180606
   # Import the NDVI to this mapset
   r.import.py Output_vegetation/L8_NDVI_30m_2018_06_06_157.tif output=ndvi_20180606
   # Check for cold pixel thresholds from corrected surface temp
   r.univar -e ts_20180606 percentile=5,10
   r.univar -e ndvi_20180606 percentile=2,5

Once you get the thresholds update the **Sheet 4: Landsat_Input** of the input excel sheet, update the **Run_py2** line number 16 to **RUN** mode and run pySEBAL again for final results.


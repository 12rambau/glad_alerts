Deforestation alert analysis
============================

this module is a 3 steps process that retrieve and compute the alert system on a selected Aera of interest.

Select an AOI
-------------

Using the provided AOI selector, select an AOI of your choice between the different methods available in the tool. We provide 3 administration descriptions (from level 0 to 2) and 3 custom shapes (drawn directly on the map, asset or shapefile). 

.. figure:: https://raw.githubusercontent.com/openforis/alert_module/master/doc/img/select_aoi.png 
    
    aoi selector 
    
.. note::

    If a custom aoi from shape or drawing is selected, you will be able to use it directly and the upload to GEE will be launched in the background
    
Retrieve the alerts
-------------------

First you'll need to select the alert system you want to use. In the current version of the module, 3 are available and will be described in the following.


.. figure:: https://raw.githubusercontent.com/openforis/alert_module/master/doc/img/alert_driver.png

    retrieve alerts

GLAD alerts
^^^^^^^^^^^

Description
###########

This data set, created by the `GLAD <http://glad.geog.umd.edu/>`_ (Global Land Analysis & Discovery) lab at the University of Maryland and supported by Global Forest Watch, is the first Landsat-based alert system for tree cover loss. While most existing loss alert products use 250-meter resolution MODIS imagery, these alerts have a 30-meter resolution and thus can detect loss at a much finer spatial scale. The alerts are currently operational for select countries in the Amazon, Congo Basin, and Southeast Asia, and will eventually be expanded to the rest of the humid tropics.

New Landsat 7 and 8 images are downloaded as they are posted online at USGS EROS, assessed for cloud cover or poor data quality, and compared to the three previous years of Landsat-derived metrics (including ranks, means, and regressions of red, infrared and shortwave bands, and ranks of NDVI, NBR, and NDWI). The metrics and the latest Landsat image are run through seven decision trees to calculate a median probability of forest disturbance. Pixels with probability >50% are reported as tree cover loss alerts. For more information on methodology, see the `paper in Environmental Research Letter` <http://iopscience.iop.org/article/10.1088/1748-9326/11/3/034008>`_.

Alerts remain unconfirmed until two or more out of four consecutive observations are labelled as tree cover loss. Alerts that remain unconfirmed for four consecutive observations or more than 180 days are removed from the data set. You can choose to view only confirmed alerts in the menu, though keep in mind that using only confirmed alerts misses the newest detections of tree cover loss.

usage
#####

Select the :code:`glad_alerts` driver and the two dates of your time range of interest. Be aware that we cannot combine results from different years as they don't live in the same dataset. The first year published on google earth engine is 2017.

.. figure:: https://raw.githubusercontent.com/openforis/alert_module/master/doc/img/glad_driver.png

    retrieve alerts

By clicking on the “run process” button, you will launch the process on you GEE account. In the information banner, the module will give you insights on the progression of the process. 

Once the process is completed you should obtain the following message:

.. figure:: https://raw.githubusercontent.com/openforis/alert_module/master/doc/img/glad_driver_done.png

    retrieve alerts

local alerts
^^^^^^^^^^^^

If you produce a locally derived alert system and you want to use it in this module you need to save it in a format compatible with the alert driver. You'll need to create to different files: 

-   the alert file: a .TIFF file with value 1 if an alert exist 0 elsewhere. You can use other value to described other type of alerts (likely etc...). Contact the contributors via the `issue panel <https://github.com/openforis/alert_module/issues/new/choose>`_ if help is needed.
-   The date file: a .TIFF file with the julian date of the alerts (number of days since 01/01/0001), 0 elsewhere. it will be used by the driver to filter the useful alerts. If you don't have this file and you want to consider every alerts in your alert file. duplicate it and replace all the non-zero by a known date (21/09/2020 = 737689). 

Once you're done, select the time range you want to study, and the two file you created.

By clicking on the “run process” button, you will launch the filtering of your alert system. In the information banner, the module will give you insights on the progression of the process.

.. figure:: https://raw.githubusercontent.com/openforis/alert_module/master/doc/img/local_driver.png

    retrieve alerts

GEE asset alerts system
^^^^^^^^^^^^^^^^^^^^^^^

If you produce or want to use a product existing on earthengine. Use the `gee asset` driver. 

You'll require 2 assets:

-   the alerts: any gee Image collection with an alert band. 1 when there is an alert 0 elsewhere 
-   the alert dates: any gee ImageCollection with an date band. the julian date of the alerts (number of days since 01/01/0001), 0 elsewhere. it will be used by the driver to filter the useful alerts. If you don't have this file and you want to consider every alerts in your alert file. duplicate it and replace all the non-zero by a known date (21/09/2020 = 737689). 

after selecting the file, you'll be asked to pick up the appropriate band.

> if you require to analyse more exotic alert system available on GEE (ex. with several bands to analyse) raise a ticket on the `issue panel <https://github.com/openforis/alert_module/issues/new/choose>`_.

Once you're done, select the time range you want to study, and the two file you created.

By clicking on the “run process” button, you will launch the filtering of your alert system. In the information banner, the module will give you insights on the progression of the process.

.. figure:: https://raw.githubusercontent.com/openforis/alert_module/master/doc/img/gee_driver.png

    retrieve alerts
    
Postprocess the alerts
----------------------

.. warning:: 

    Before launching the process, make sure that your instance is powerful enough to run the process. `m4` is the minimum for country-size computation.

By clicking on `run postprocessing`, you will launch the analysis of the glad alerts on your SEPAL computer. The different steps are described here:

-   Merge the tiles to produce a single raster (.tif) 
-   Delete the downloaded tiles
-   Create patches of glad alerts in a tmp file
-   Produce a distribution of the glad alert patches 

.. warning:: 

    This action is performed in your sepal computer, you don't want to close the Sepal module before it's finished.

.. figure:: https://raw.githubusercontent.com/openforis/alert_module/master/doc/img/postprocess.png

    postprocessing

Use the results
---------------

The outputs are automatically generated after the postprocessing.
After finishing your analysis, the module will give you several outputs that can be displayed in the `Result` tile.  
You can see here an example of the results obtained on Singapore for 2020

.. figure:: https://raw.githubusercontent.com/openforis/alert_module/master/doc/img/results.png

    results

on the left you have the distribution of the confirmed Glad alerts (weighted by their size). On the map you can observe the AOI boundaries in blue and the alerts in yellow (potential) and red (confirmed). This map is fully interactive.  
By clicking on the top buttons, you can obtain the files used to display the results: 

-   The raster of the glad alerts clipped on the AOI
-   The csv file of the alerts on the AOI, separated between confirmed and potential
-   The plot of the distribution of the alerts
 
.. note:: 

    **Tips:** Remember that all your results output have been save on your sepal account and live in ~/module_results/deforestation_alert/[aoi_name]/

.. figure:: https://raw.githubusercontent.com/openforis/alert_module/master/doc/img/download_hist.png

    histogram


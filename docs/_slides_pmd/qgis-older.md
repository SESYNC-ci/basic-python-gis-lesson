---
---

## QGIS

QGIS is a software package that does its very best to ease the process of navigating, editing and processing spatial data.
As we will see, it is built on top of the open source libraries and it too is free to distribute and enhance.

It's a GUI too, so it's time to explore!

Access the PostgreSQL server to vizualize the location of US Highways.

Name
: postGIS
Host
: pgstudio.research.sesync.org
Port
: 5432
Database
: postgis_in_action
Username
: student
Password
: ******

We are going to calculate the length of US Route 50, all of its segments put together.

In the "Select features using expression" window, we'll use:

~~~
"name" like '%US Route 50%'
~~~

## PyQGIS

When you initiate a Python Console from within QGIS, the python environment is primed for interactive work with all aspects of the GIS software.

[//]: # " ~~~ "
[//]: # " from qgis.core import * "
[//]: # " import qgis.utils "
[//]: # " ~~~ "

Entering the following commands in the script editor, not directly in the console, because we're going to use them again.

~~~
# Mac
data = '%sandbox%/data/'

# # Windows
# data = '%sandbox\data\\'

tif = 'westernfires_vir_2015231_geo.tif'
tif_name = 'vir'
rlayer = iface.addRasterLayer(data + tif, tif_name)
~~~

We are looking at the night sky, over the western United States, as seen by the Visible Infrared Imaging Radiometer Suite (VIIRS) sensor.
We are going to embark on the quest of attempting to say which areas lighting up this map are cities, the rest are a group of wildfires that were often in the news last summer.

Open a new project, so we can look at something else:

~~~
shp = 'ne_10m_urban_areas'
shp_name = 'urban_areas'
vlayer = iface.addVectorLayer(data + shp, shp_name, 'ogr')
~~~

This is a global extent dataset on urban areas, and it is a vector data type rather than a raster.

~~~
vlayer.selectedFeatureCount()
~~~

~~~
for feature in vlayer.selectedFeatures():
  geom = feature.geometry()
  id = feature.id()
  ct = geom.centroid().asPoint()
  print "Feature ID: %s, Centroid: %s" % (id, ct)
~~~

~~~
for feature in vlayer.selectedFeatures():
  geom = feature.geometry()
  id = feature.id()
  sr = feature['scalerank']
  print "Feature ID: %s, Scale rank: %s" % (id, sr)
~~~

Open a new project, and we'll look at both of these together.
Run the full script you've typed up so far.

QGIS has now conveniently layered on top of the raster our shapefile defining outlines of urban areas.

Question
: From what we know about these datasets, or can learn using GDAL, should we be surprised that these line up?

On the fly transformation is a blessing and a curse.
You have to be aware of whether the tools you use within QGIS are actually using the "on the fly" projection or the projection of the data source.

~~~
import qgis.analysis as qa
calc = qa.QgsZonalStatistics(vlayer, rlayer.source(), "b1_",
	                         1, qa.QgsZonalStatistics.Mean)
calc.calculateStatistics(None)
~~~

Find the attribute table for the vector layer to see what we've achieved.

Question
: If we don't want an "OTF" transformation, do we now know a tool to make the transformation "on the *file*".

Replace the code above to get your transformed raster.

~~~
tif = 'westernfires_vir_2015231_geo-wgs84.tif'
tif_name = 'vir'
rlayer = iface.addRasterLayer(data + tif, tif_name)
~~~

Notice the OTF indicator hasn't come up.

Question
: What ideas do we have to use these two datasets to find the wildfires? In other words, what's a strategy for "zeroing" out the pixels in the raster dataset that fall within these polygons?

Step 1 is to give our urban areas a little wiggle room.

~~~
msk_shp = 'urban_area_mask.shp'
qa.QgsGeometryAnalyzer().buffer(vlayer, data + msk_shp, 0.1, False, False, -1)
~~~

Step 2 is to convert the enlarged polygons to a raster that will serve as a mask: it only has zeros and ones.

~~~
import os

msk_tif = 'urban_area_mask.tif'
sys_call = ' '.join([
    'gdal_rasterize -ot Byte -burn 1 -tr 0.01 0.01 -l',
    msk_shp[:-4],
    data + msk_shp,
    data + msk_tif
	])
os.system(sys_call)
msk = iface.addRasterLayer(data + msk_tif, "uam")
~~~

Could we use a PyQGIS method to achieve this -- yes, we probably could.
What the method will be, however, is a "wrapper" to the underlying GDAL utility.
Sometimes a system call is the only way to proceed if you can't find a "wrapper" or find that it's broken.

[//]: # " FIXME Make these the same number of pixels "

A raster calculator is just a way of doing algebra that's forgiving of our poorly aligned matrices.

[//]: # " FIXME: read PostGIS "

[//]: # " FIXME: write/import PostGIS "

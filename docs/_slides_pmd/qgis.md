---
---

## QGIS

QGIS is a software package that does its very best to ease the process of navigating, editing and processing spatial data.
It is built on top of the open source libraries and it too is free to distribute and enhance.

===

## PyQGIS

When you initiate a Python Console from within QGIS, the python environment is primed for interactive work with all aspects of the GIS software.

Outside the QGIS environment, you must jump through some hoops to get started. The [PyQGIS Cookbook](https://docs.qgis.org/testing/en/docs/pyqgis_developer_cookbook/) refers to these as "standalone-scripts".

- know and append the installation paths
- disable any GUI side-effects

```{python, title = "{{ site.handouts[0] }}"}
import os, sys
os.environ['QT_QPA_PLATFORM'] = 'offscreen'
sys.path.append('/usr/share/qgis/python/plugins')
```

===

The pipeline for any processing begins with
initializing a `QgsApplication`.


```{python, title = "{{ site.handouts[0] }}"}
from qgis.core import *

QgsApplication.setPrefixPath('/usr', True)
qgs = QgsApplication([], False)
qgs.initQgis()
```

===

If you want to use QGIS to see maps, you better use their
interface for now, but we have access to the basic reading
and writing that QGIS provides.

```{python, title = "{{ site.handouts[0] }}"}
vlayer = QgsVectorLayer('/data/huc250k', 'huc250', 'ogr')
feature = next(vlayer.getFeatures())
feature
```

This is sometimes unsatisfying ...

===

Feature attributes are present in a Python dictionary.

```{python, title = "{{ site.handouts[0] }}"}
feature.fields().names()
feature.attributes()
feature['HUC_NAME']
```

===

Feature geometry is represented as a different kind of
object than present in a `GeoDataFrame`, but through WKT
or WKB it would be possible to translate.

```python
feature.geometry().asWkt()
```

===

The Python API supports the creation of vector data and a host of algorithms built in, or installed as plugins, with the QGIS GUI.

```{python, title = "{{ site.handouts[0] }}"}
sesync = QgsPointXY(1661514.580789013, 1943320.104999293)

sindex = QgsSpatialIndex(vlayer.getFeatures())
huc = sindex.nearestNeighbor(sesync, 5)
```

===

```python
huc
```

===

```{python, title = "{{ site.handouts[0] }}"}
for id in huc:
    feature = vlayer.getFeature(id)
    print(feature['HUC_NAME'])
```

===

The processing algorithms come from several providers, QGIS itself, alsong with GDAL, GRASS and possibly others.

```{python, title = "{{ site.handouts[0] }}"}
from processing.core.Processing import Processing
Processing.initialize()
qgs.processingRegistry().algorithms()
```

===

```{python, title = "{{ site.handouts[0] }}"}
from processing.tools import general
general.algorithmHelp('gdal:dissolve')
```

===

```{python, title = "{{ site.handouts[0] }}"}
# import processing

# params = {
#    'INPUT': vlayer,
#    'FIELD': 'ACC',
#    'OUTPUT': out_vlayer}
# processing.run('gdal:dissolve', params, context = None)
```

====

```{python, title = "{{ site.handouts[0] }}"}
# from qgis.analysis import QgsZonalStatistics

# fileInfo = QFileInfo('/data/agg_3_r_nlcd2001_Houston.tiff')
# rlayer = QgsRasterLayer('/data/agg_3_r_nlcd2001_Houston.tiff', fileInfo.baseName())
# calc = QgsZonalStatistics(vlayer, rlayer, '', 1, qa.QgsZonalStatistics.Mean)
# result = calc.calculateStatistics(None)
```

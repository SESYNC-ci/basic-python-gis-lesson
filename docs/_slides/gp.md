---
---

## GeoPandas

The object for managing simple feature collections in Python is the `GeoDataFrame`.


~~~python
import geopandas as gpd

gdf = gpd.read_file('/data/cb_2016_us_county_5m')
gdf.head()
~~~
{:.text-document title="{{ site.handouts[0] }}"}

~~~
Out[1]: 
  STATEFP COUNTYFP  COUNTYNS        AFFGEOID  GEOID       NAME LSAD  \
0      04      015  00025445  0500000US04015  04015     Mohave   06   
1      12      035  00308547  0500000US12035  12035    Flagler   06   
2      20      129  00485135  0500000US20129  20129     Morton   06   
3      28      093  00695770  0500000US28093  28093   Marshall   06   
4      29      510  00767557  0500000US29510  29510  St. Louis   25   

         ALAND     AWATER                                           geometry  
0  34475567011  387344307  POLYGON ((-114.755618 36.087166, -114.753638 3...  
1   1257365642  221047161  POLYGON ((-81.52365999999999 29.622432, -81.32...  
2   1889993251     507796  POLYGON ((-102.041952 37.024742, -102.04195 37...  
3   1828989833    9195190  POLYGON ((-89.7243244282036 34.9952117286505, ...  
4    160458044   10670040  POLYGON ((-90.318212 38.600017, -90.301828 38....  
~~~
{:.output}



===

The "query" operator allows filtering records of a [pandas](){:.pylib} table, and all the usual
table operations cary through. The "plot" method draws a map by default.


~~~python
md = gdf.query('STATEFP == "24"')
md.plot()
~~~
{:.text-document title="{{ site.handouts[0] }}"}

~~~
Out[1]: <matplotlib.axes._subplots.AxesSubplot at 0x7fedb55310b8>
~~~
{:.output}

![plot of ../images/gp_figure2_1.png]({{ site.baseurl }}/images/gp_figure2_1.png)

===

Reprojections performed "on the fly" (without writing to disk) require a PROJ4 string or, using the `epsg` argument, a numeric code.


~~~python
proj4 = '+proj=aea +lat_1=29.5 +lat_2=45.5 +lat_0=23 +lon_0=-96 +x_0=0 +y_0=0 +ellps=GRS80 +towgs84=0,0,0,0,0,0,0 +units=m +no_defs'
md = md.to_crs(proj4)
md.plot()
~~~
{:.text-document title="{{ site.handouts[0] }}"}

~~~
Out[1]: <matplotlib.axes._subplots.AxesSubplot at 0x7fedb52da3c8>
~~~
{:.output}

![plot of ../images/gp_figure3_1.png]({{ site.baseurl }}/images/gp_figure3_1.png)

===

The usual geospatial metadata have predictiable methods. The `dir` function returns a list of all the methods and attributes an object has in its class definition.


~~~python
md.bounds.head()
~~~
{:.input}
~~~
Out[1]: 
              minx          miny          maxx          maxy
941   1.635263e+06  1.966733e+06  1.653825e+06  1.986272e+06
1034  1.674433e+06  1.930300e+06  1.721151e+06  1.986186e+06
1035  1.643875e+06  1.843402e+06  1.698630e+06  1.888419e+06
1120  1.667118e+06  1.995332e+06  1.714008e+06  2.037741e+06
1206  1.630504e+06  1.913300e+06  1.670057e+06  1.969954e+06
~~~
{:.output}



~~~python
dir(md)
~~~
{:.input}
~~~
Out[1]: 
['AFFGEOID',
 'ALAND',
 'AWATER',
 'COUNTYFP',
 'COUNTYNS',
 'GEOID',
 'LSAD',
 'NAME',
 'STATEFP',
 'T',
 '_AXIS_ALIASES',
 '_AXIS_IALIASES',
 '_AXIS_LEN',
 '_AXIS_NAMES',
 '_AXIS_NUMBERS',
 '_AXIS_ORDERS',
 '_AXIS_REVERSED',
 '_AXIS_SLICEMAP',
 '__abs__',
 '__add__',
 '__and__',
 '__array__',
 '__array_wrap__',
 '__bool__',
 '__bytes__',
 '__class__',
 '__contains__',
 '__copy__',
 '__deepcopy__',
 '__delattr__',
 '__delitem__',
 '__dict__',
 '__dir__',
 '__div__',
 '__doc__',
 '__eq__',
 '__finalize__',
 '__floordiv__',
 '__format__',
 '__ge__',
 '__geo_interface__',
 '__getattr__',
 '__getattribute__',
 '__getitem__',
 '__getstate__',
 '__gt__',
 '__hash__',
 '__iadd__',
 '__iand__',
 '__ifloordiv__',
 '__imod__',
 '__imul__',
 '__init__',
 '__invert__',
 '__ior__',
 '__ipow__',
 '__isub__',
 '__iter__',
 '__itruediv__',
 '__ixor__',
 '__le__',
 '__len__',
 '__lt__',
 '__mod__',
 '__module__',
 '__mul__',
 '__ne__',
 '__neg__',
 '__new__',
 '__nonzero__',
 '__or__',
 '__pow__',
 '__radd__',
 '__rand__',
 '__rdiv__',
 '__reduce__',
 '__reduce_ex__',
 '__repr__',
 '__rfloordiv__',
 '__rmod__',
 '__rmul__',
 '__ror__',
 '__round__',
 '__rpow__',
 '__rsub__',
 '__rtruediv__',
 '__rxor__',
 '__setattr__',
 '__setitem__',
 '__setstate__',
 '__sizeof__',
 '__str__',
 '__sub__',
 '__subclasshook__',
 '__truediv__',
 '__unicode__',
 '__weakref__',
 '__xor__',
 '_accessors',
 '_add_numeric_operations',
 '_add_series_only_operations',
 '_add_series_or_dataframe_operations',
 '_agg_by_level',
 '_agg_doc',
 '_aggregate',
 '_aggregate_multiple_funcs',
 '_align_frame',
 '_align_series',
 '_apply_broadcast',
 '_apply_empty_result',
 '_apply_raw',
 '_apply_standard',
 '_at',
 '_box_col_values',
 '_box_item_values',
 '_builtin_table',
 '_check_inplace_setting',
 '_check_is_chained_assignment_possible',
 '_check_percentile',
 '_check_setitem_copy',
 '_clear_item_cache',
 '_clip_with_one_bound',
 '_clip_with_scalar',
 '_combine_const',
 '_combine_frame',
 '_combine_match_columns',
 '_combine_match_index',
 '_combine_series',
 '_combine_series_infer',
 '_compare_frame',
 '_compare_frame_evaluate',
 '_consolidate',
 '_consolidate_inplace',
 '_construct_axes_dict',
 '_construct_axes_dict_for_slice',
 '_construct_axes_dict_from',
 '_construct_axes_from_arguments',
 '_constructor',
 '_constructor_expanddim',
 '_constructor_sliced',
 '_convert',
 '_count_level',
 '_create_indexer',
 '_cx',
 '_cython_table',
 '_deprecations',
 '_dir_additions',
 '_dir_deletions',
 '_drop_axis',
 '_ensure_valid_index',
 '_expand_axes',
 '_flex_compare_frame',
 '_from_arrays',
 '_from_axes',
 '_generate_sindex',
 '_geometry_column_name',
 '_get_agg_axis',
 '_get_axis',
 '_get_axis_name',
 '_get_axis_number',
 '_get_axis_resolvers',
 '_get_block_manager_axis',
 '_get_bool_data',
 '_get_cacher',
 '_get_geometry',
 '_get_index_resolvers',
 '_get_item_cache',
 '_get_numeric_data',
 '_get_valid_indices',
 '_get_value',
 '_get_values',
 '_getitem_array',
 '_getitem_column',
 '_getitem_frame',
 '_getitem_multilevel',
 '_getitem_slice',
 '_gotitem',
 '_iat',
 '_iget_item_cache',
 '_iloc',
 '_indexed_same',
 '_info_axis',
 '_info_axis_name',
 '_info_axis_number',
 '_info_repr',
 '_init_dict',
 '_init_mgr',
 '_init_ndarray',
 '_internal_names',
 '_internal_names_set',
 '_invalidate_sindex',
 '_is_builtin_func',
 '_is_cached',
 '_is_cython_func',
 '_is_datelike_mixed_type',
 '_is_mixed_type',
 '_is_numeric_mixed_type',
 '_is_view',
 '_ix',
 '_ixs',
 '_join_compat',
 '_loc',
 '_maybe_cache_changed',
 '_maybe_update_cacher',
 '_metadata',
 '_needs_reindex_multi',
 '_obj_with_exclusions',
 '_protect_consolidate',
 '_reduce',
 '_reindex_axes',
 '_reindex_axis',
 '_reindex_columns',
 '_reindex_index',
 '_reindex_multi',
 '_reindex_with_indexers',
 '_repr_data_resource_',
 '_repr_fits_horizontal_',
 '_repr_fits_vertical_',
 '_repr_html_',
 '_repr_latex_',
 '_reset_cache',
 '_reset_cacher',
 '_sanitize_column',
 '_selected_obj',
 '_selection',
 '_selection_list',
 '_selection_name',
 '_series',
 '_set_as_cached',
 '_set_axis',
 '_set_axis_name',
 '_set_geometry',
 '_set_is_copy',
 '_set_item',
 '_set_value',
 '_setitem_array',
 '_setitem_frame',
 '_setitem_slice',
 '_setup_axes',
 '_shallow_copy',
 '_sindex',
 '_sindex_generated',
 '_slice',
 '_stat_axis',
 '_stat_axis_name',
 '_stat_axis_number',
 '_take',
 '_to_dict_of_blocks',
 '_to_geo',
 '_try_aggregate_string_function',
 '_typ',
 '_unpickle_frame_compat',
 '_unpickle_matrix_compat',
 '_update_inplace',
 '_validate_dtype',
 '_values',
 '_where',
 '_xs',
 'abs',
 'add',
 'add_prefix',
 'add_suffix',
 'agg',
 'aggregate',
 'align',
 'all',
 'any',
 'append',
 'apply',
 'applymap',
 'area',
 'as_matrix',
 'asfreq',
 'asof',
 'assign',
 'astype',
 'at',
 'at_time',
 'axes',
 'between_time',
 'bfill',
 'bool',
 'boundary',
 'bounds',
 'boxplot',
 'buffer',
 'cascaded_union',
 'centroid',
 'clip',
 'clip_lower',
 'clip_upper',
 'columns',
 'combine',
 'combine_first',
 'compound',
 'contains',
 'convex_hull',
 'copy',
 'corr',
 'corrwith',
 'count',
 'cov',
 'crosses',
 'cummax',
 'cummin',
 'cumprod',
 'cumsum',
 'cx',
 'describe',
 'diff',
 'difference',
 'disjoint',
 'dissolve',
 'distance',
 'div',
 'divide',
 'dot',
 'drop',
 'drop_duplicates',
 'dropna',
 'dtypes',
 'duplicated',
 'empty',
 'envelope',
 'eq',
 'equals',
 'eval',
 'ewm',
 'expanding',
 'explode',
 'exterior',
 'ffill',
 'fillna',
 'filter',
 'first',
 'first_valid_index',
 'floordiv',
 'from_dict',
 'from_features',
 'from_file',
 'from_items',
 'from_postgis',
 'from_records',
 'ftypes',
 'ge',
 'geom_almost_equals',
 'geom_equals',
 'geom_equals_exact',
 'geom_type',
 'geometry',
 'get',
 'get_dtype_counts',
 'get_ftype_counts',
 'get_values',
 'groupby',
 'gt',
 'head',
 'hist',
 'iat',
 'idxmax',
 'idxmin',
 'iloc',
 'index',
 'infer_objects',
 'info',
 'insert',
 'interiors',
 'interpolate',
 'intersection',
 'intersects',
 'is_copy',
 'is_empty',
 'is_ring',
 'is_simple',
 'is_valid',
 'isin',
 'isna',
 'isnull',
 'items',
 'iterfeatures',
 'iteritems',
 'iterrows',
 'itertuples',
 'ix',
 'join',
 'keys',
 'kurt',
 'kurtosis',
 'last',
 'last_valid_index',
 'le',
 'length',
 'loc',
 'lookup',
 'lt',
 'mad',
 'mask',
 'max',
 'mean',
 'median',
 'melt',
 'memory_usage',
 'merge',
 'min',
 'mod',
 'mode',
 'mul',
 'multiply',
 'ndim',
 'ne',
 'nlargest',
 'notna',
 'notnull',
 'nsmallest',
 'nunique',
 'overlaps',
 'pct_change',
 'pipe',
 'pivot',
 'pivot_table',
 'plot',
 'pop',
 'pow',
 'prod',
 'product',
 'project',
 'quantile',
 'query',
 'radd',
 'rank',
 'rdiv',
 'reindex',
 'reindex_axis',
 'reindex_like',
 'relate',
 'rename',
 'rename_axis',
 'reorder_levels',
 'replace',
 'representative_point',
 'resample',
 'reset_index',
 'rfloordiv',
 'rmod',
 'rmul',
 'rolling',
 'rotate',
 'round',
 'rpow',
 'rsub',
 'rtruediv',
 'sample',
 'scale',
 'select',
 'select_dtypes',
 'sem',
 'set_axis',
 'set_geometry',
 'set_index',
 'shape',
 'shift',
 'simplify',
 'sindex',
 'size',
 'skew',
 'slice_shift',
 'sort_index',
 'sort_values',
 'squeeze',
 'stack',
 'std',
 'style',
 'sub',
 'subtract',
 'sum',
 'swapaxes',
 'swaplevel',
 'symmetric_difference',
 'tail',
 'take',
 'to_clipboard',
 'to_crs',
 'to_csv',
 'to_dense',
 'to_dict',
 'to_excel',
 'to_feather',
 'to_file',
 'to_gbq',
 'to_hdf',
 'to_html',
 'to_json',
 'to_latex',
 'to_msgpack',
 'to_panel',
 'to_parquet',
 'to_period',
 'to_pickle',
 'to_records',
 'to_sparse',
 'to_sql',
 'to_stata',
 'to_string',
 'to_timestamp',
 'to_xarray',
 'total_bounds',
 'touches',
 'transform',
 'translate',
 'transpose',
 'truediv',
 'truncate',
 'tshift',
 'type',
 'tz_convert',
 'tz_localize',
 'unary_union',
 'union',
 'unstack',
 'update',
 'values',
 'var',
 'where',
 'within',
 'xs']
~~~
{:.output}



~~~python
?md.geom_type
~~~
{:.input}
~~~
[0;31mType:[0m        property
[0;31mString form:[0m <property object at 0x7fedc0c4b458>
[0;31mDocstring:[0m   Return the geometry type of each geometry in the GeoSeries
~~~
{:.output}



===

Create geometries with the [shapely](){:.pylib} package, which underlies the 
`GeoSeries` column of a `GeoDataFrame`.


~~~python
from shapely.geometry import Point

point = Point(-76.505206, 38.9767231)
geoSeries = gpd.GeoSeries(point, crs = {'init': 'epsg:4326'})
~~~
{:.text-document title="{{ site.handouts[0] }}"}



===

The `GeoDataFrame` can be thought of as a simple feature collection, where the rows represent a single geometry (including a "Multi\*"). Use the "geometry" keyword argument to construct the feature collection from scratch.


~~~python
import pandas as pd

df = pd.DataFrame({'name':['SESYNC']})
sesync = gpd.GeoDataFrame(df, geometry = geoSeries)
sesync = sesync.to_crs(proj4)
sesync.geometry[0].wkt
~~~
{:.input}
~~~
Out[1]: 'POINT (1661514.580789013 1943320.104999293)'
~~~
{:.output}



===

The [matplotlib](){:.pylib} package provides finer control over plotting; for instance, when we want to draw multiple times on the same axes.


~~~python
import matplotlib.pyplot as plt

fig, ax = plt.subplots()
ax.set_aspect('equal')
md.plot(ax=ax, column='ALAND')
sesync.plot(ax=ax, color='red', markersize=16)
~~~
{:.text-document title="{{ site.handouts[0] }}"}

~~~
Out[1]: <matplotlib.axes._subplots.AxesSubplot at 0x7fedb5242240>
~~~
{:.output}

![plot of ../images/gp_figure9_1.png]({{ site.baseurl }}/images/gp_figure9_1.png)

===


~~~python
md_state = md.dissolve(by='STATEFP')
md_state.plot(color='white', edgecolor = 'black')
~~~
{:.text-document title="{{ site.handouts[0] }}"}

~~~
Out[1]: <matplotlib.axes._subplots.AxesSubplot at 0x7fedb51c1ba8>
~~~
{:.output}

![plot of ../images/gp_figure10_1.png]({{ site.baseurl }}/images/gp_figure10_1.png)

===


~~~python
huc = gpd.read_file('/data/huc250k')
huc_sindex = huc.sindex
~~~
{:.text-document title="{{ site.handouts[0] }}"}



===


~~~python
huc_sindex
~~~
{:.input}
~~~
Out[1]: <geopandas.sindex.SpatialIndex at 0x7fedb5177f98>
~~~
{:.output}



===


~~~python
md_state.crs = md.crs
md_bounds = md_state.to_crs(huc.crs).unary_union.bounds
huc_md_id = huc_sindex.intersection(md_bounds)
huc_md = huc.iloc[list(huc_md_id)]
huc_md = huc_md.to_crs(proj4)
~~~
{:.text-document title="{{ site.handouts[0] }}"}




~~~python
huc_md.head()
~~~
{:.input}
~~~
Out[1]: 
              AREA      PERIMETER  HUC250K_  HUC250K_ID  HUC_CODE  \
936   5.910075e+09  503796.525429       938         948  02070004   
974   2.502119e+09  252945.829646       976         987  02070009   
953   1.932740e+09  344024.043730       955         966  02040204   
1013  7.655817e+06   16202.881690      1015        1025  02060003   
954   2.706198e+09  329368.895196       956         967  02040206   

                   HUC_NAME REG   SUB     ACC       CAT  \
936   Conococheague-Opequon  02  0207  020700  02070004   
974                Monocacy  02  0207  020700  02070009   
953            Delaware Bay  02  0204  020402  02040204   
1013     Gunpowder-Patapsco  02  0206  020600  02060003   
954        Cohansey-Maurice  02  0204  020402  02040206   

                                               geometry  
936   POLYGON ((1546272.399986326 2037042.997213009,...  
974   POLYGON ((1610556.9351812 2016042.919984103, 1...  
953   POLYGON ((1734088.560572327 2052630.260708987,...  
1013  POLYGON ((1686399.495631644 2001319.939650947,...  
954   POLYGON ((1768156.067673573 2051851.569199119,...  
~~~
{:.output}



===


~~~python
fig, ax = plt.subplots()
ax.set_aspect('equal')
md_state.plot(ax = ax, color = 'none', edgecolor = 'black')
huc_md.plot(ax = ax, color = 'none', edgecolor = 'blue')
~~~
{:.text-document title="{{ site.handouts[0] }}"}

~~~
Out[1]: <matplotlib.axes._subplots.AxesSubplot at 0x7fedb5260d30>
~~~
{:.output}

![plot of ../images/gp_figure15_1.png]({{ site.baseurl }}/images/gp_figure15_1.png)

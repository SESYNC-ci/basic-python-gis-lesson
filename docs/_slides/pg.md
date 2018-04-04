---
---

## Write to Database


~~~python
import psycopg2
import shapely
~~~
{:.text-document title="{{ site.handouts[0] }}"}



~~~pql
CREATE TABLE huc (
  AFFGEOID VARCHAR PRIMARY KEY,
  STATEFP CHAR(2),
  COUNTYFP CHAR(3)
);
SELECT AddGeometryColumn('public','huc','geom', 5071, 'POLYGON', 2);
~~~

~~~psql
CREATE INDEX huc_index
  ON huc
  USING GIST(geom)
~~~


~~~python
connection = psycopg2.connect(database = 'huc250')
cursor = connection.cursor()
~~~
{:.text-document title="{{ site.handouts[0] }}"}



===


~~~python
sql = """
INSERT INTO huc (affgeoid, statefp, countyfp, geom)
VALUES ('{AFFGEOID}', '{STATEFP}', '{COUNTYFP}', ST_GeomFromText('{wkt}', 5071))
"""
row = gdf.loc[...]
wkt = row['geometry'].to_wkt()
cursor.execute(sql.format(wkt = wkt, **row))
connection.commit()
~~~
{:.text-document title="{{ site.handouts[0] }}"}



===


~~~python
huc = gpd.read_postgis('SELECT * FROM huc', con = connection, geom_col = 'geom')
huc.crs = proj4
connection.close()
~~~
{:.text-document title="{{ site.handouts[0] }}"}

~~~
[0;31m---------------------------------------------------------------------------[0m
[0;31mNameError[0m                                 Traceback (most recent call last)
[0;32m<ipython-input-1-bfa1d44c4a53>[0m in [0;36m<module>[0;34m()[0m
[1;32m      1[0m [0;34m[0m[0m
[0;32m----> 2[0;31m [0mhuc[0m [0;34m=[0m [0mgpd[0m[0;34m.[0m[0mread_postgis[0m[0;34m([0m[0;34m'SELECT * FROM huc'[0m[0;34m,[0m [0mcon[0m [0;34m=[0m [0mconnection[0m[0;34m,[0m [0mgeom_col[0m [0;34m=[0m [0;34m'geom'[0m[0;34m)[0m[0;34m[0m[0m
[0m[1;32m      3[0m [0mhuc[0m[0;34m.[0m[0mcrs[0m [0;34m=[0m [0mproj4[0m[0;34m[0m[0m
[1;32m      4[0m [0mconnection[0m[0;34m.[0m[0mclose[0m[0;34m([0m[0;34m)[0m[0;34m[0m[0m

[0;31mNameError[0m: name 'gpd' is not defined
~~~
{:.output}



===


~~~python
for idx, row in gdf.iterrows():
  wkt = row['geometry'].to_wkt()
  cursor.execute(sql.format(wkt = wkt, **row))
connection.close()
~~~
{:.text-document title="{{ site.handouts[0] }}"}




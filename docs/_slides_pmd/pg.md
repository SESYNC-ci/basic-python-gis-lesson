---
---

## Write to Database

```{python, title = "{{ site.handouts[0] }}"}
import psycopg2
import shapely
```

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

```{python, title = "{{ site.handouts[0] }}"}
connection = psycopg2.connect(database = 'huc250')
cursor = connection.cursor()
```

===

```{python, title = "{{ site.handouts[0] }}", evaluate = False}
sql = """
INSERT INTO huc (affgeoid, statefp, countyfp, geom)
VALUES ('{AFFGEOID}', '{STATEFP}', '{COUNTYFP}', ST_GeomFromText('{wkt}', 5071))
"""
row = gdf.loc[...]
wkt = row['geometry'].to_wkt()
cursor.execute(sql.format(wkt = wkt, **row))
connection.commit()
```

===

```{python, title = "{{ site.handouts[0] }}"}
huc = gpd.read_postgis('SELECT * FROM huc', con = connection, geom_col = 'geom')
huc.crs = proj4
connection.close()
```

===

```{python, title = "{{ site.handouts[0] }}", evaluate = False}
for idx, row in gdf.iterrows():
  wkt = row['geometry'].to_wkt()
  cursor.execute(sql.format(wkt = wkt, **row))
connection.close()
```


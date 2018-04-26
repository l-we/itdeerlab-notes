


wget http://files.cnblogs.com/files/think8848/osmsld.zip

[root@demo tmp]# chmod +x osmsld/*
[root@demo tmp]# ll osmsld
total 172
-rwxr-xr-x. 1 root root   9838 Oct 30  2016 create_tables.sql
-rwxr-xr-x. 1 root root   7394 Oct 30  2016 create_views.sql
-rwxr-xr-x. 1 root root    866 Oct 30  2016 drop_tables.sql
-rwxr-xr-x. 1 root root    842 Oct 30  2016 drop_views.sql
-rwxr-xr-x. 1 root root    807 Oct 30  2016 layergroup.xml
-rwxr-xr-x. 1 root root   1641 Oct 30  2016 SLD_create.sh
-rwxr-xr-x. 1 root root    758 Oct 30  2016 SLD_delete.sh
-rwxr-xr-x. 1 root root 134101 Oct 30  2016 sld.zip
[root@demo tmp]# 
[root@demo tmp]# su postgres


```
bash-4.2$ psql -U demo -h 127.0.0.1 -p 5432 -W -d demogisdb -a -f /tmp/osmsld/create_tables.sql
Password for user demo: 
DROP TABLE IF EXISTS "aero-poly";
psql:/tmp/osmsld/create_tables.sql:1: NOTICE:  table "aero-poly" does not exist, skipping
DROP TABLE
CREATE TABLE "aero-poly" AS ( 
  SELECT way,aeroway 
  FROM planet_osm_polygon
  WHERE "aeroway" IN ('apron','runway','taxiway','helipad')
  ORDER BY z_order ASC );
SELECT 1113
CREATE INDEX "aero-poly_way_idx" ON "aero-poly" USING gist (way);
CREATE INDEX
DROP TABLE IF EXISTS "agriculture";
psql:/tmp/osmsld/create_tables.sql:9: NOTICE:  table "agriculture" does not exist, skipping
DROP TABLE
CREATE TABLE "agriculture" AS ( 
  SELECT way,landuse FROM planet_osm_polygon
  WHERE "landuse" IN ('allotments','farm','farmland','farmyard',
                      'orchard','vineyard')
  ORDER BY z_order ASC );
SELECT 15930
CREATE INDEX "agriculture_way_idx" ON "agriculture" USING gist (way);
CREATE INDEX
DROP TABLE IF EXISTS "amenity-areas";
psql:/tmp/osmsld/create_tables.sql:17: NOTICE:  table "amenity-areas" does not exist, skipping
DROP TABLE
CREATE TABLE "amenity-areas" AS (
  SELECT way,amenity FROM planet_osm_polygon
  WHERE amenity IN ('hospital','college','school','university') 
  AND (building IS NULL OR building NOT IN ('no'))
);
SELECT 16893
CREATE INDEX "amenity-areas_way_idx" ON "amenity-areas" USING gist (way);
CREATE INDEX
DROP TABLE IF EXISTS "beach";
psql:/tmp/osmsld/create_tables.sql:25: NOTICE:  table "beach" does not exist, skipping
DROP TABLE
CREATE TABLE "beach" AS ( 
  SELECT way,"natural"
  FROM planet_osm_polygon
  WHERE "natural" = 'beach'
  ORDER BY z_order ASC
);
SELECT 902
CREATE INDEX "beach_way_idx" ON "beach" USING gist (way);
CREATE INDEX
DROP TABLE IF EXISTS "building";
psql:/tmp/osmsld/create_tables.sql:34: NOTICE:  table "building" does not exist, skipping
DROP TABLE
CREATE TABLE "building" AS ( 
  SELECT way,building,aeroway
  FROM planet_osm_polygon
  WHERE ("building" IS NOT NULL
  AND "building" != 'no')
  OR "aeroway" IN ('terminal')
  ORDER BY z_order ASC 
);
SELECT 923245
CREATE INDEX "building_way_idx" ON "building" USING gist (way);
CREATE INDEX
DROP TABLE IF EXISTS "forest";
psql:/tmp/osmsld/create_tables.sql:45: NOTICE:  table "forest" does not exist, skipping
DROP TABLE
CREATE TABLE "forest" AS ( SELECT *, (CASE 
            WHEN way_area >= 10000000 THEN 'huge'
            WHEN way_area >= 1000000 THEN 'large'
            WHEN way_area >= 100000 THEN 'medium'
            ELSE 'small' END) AS size FROM planet_osm_polygon
          WHERE "natural" IN ('wood') OR "landuse" IN ('forest','wood')
          ORDER BY z_order ASC );
SELECT 76620
CREATE INDEX "forest_way_idx" ON "forest" USING gist (way);
CREATE INDEX
DROP TABLE IF EXISTS "grass";
psql:/tmp/osmsld/create_tables.sql:55: NOTICE:  table "grass" does not exist, skipping
DROP TABLE
CREATE TABLE "grass" AS ( SELECT way,landuse, (CASE 
            WHEN way_area >= 10000000 THEN 'huge'
            WHEN way_area >= 1000000 THEN 'large'
            WHEN way_area >= 100000 THEN 'medium'
            ELSE 'small' END) AS size FROM planet_osm_polygon
          WHERE "landuse" IN ('grass', 'greenfield', 'meadow')
            OR "natural" IN ('fell', 'heath', 'scrub')
          ORDER BY z_order ASC 
        );
SELECT 22617
CREATE INDEX "grass_way_idx" ON "grass" USING gist (way);
CREATE INDEX
DROP TABLE IF EXISTS "highway-label";
psql:/tmp/osmsld/create_tables.sql:67: NOTICE:  table "highway-label" does not exist, skipping
DROP TABLE
CREATE TABLE "highway-label" AS (
  SELECT way,name,highway, 
    CASE WHEN oneway IN ('yes','true','1') THEN 'yes'::text END AS oneway
  FROM planet_osm_line
  WHERE "highway" IS NOT NULL
  AND ("name" IS NOT NULL OR "oneway" IS NOT NULL)
  ORDER BY z_order ASC 
);
SELECT 770804
CREATE INDEX "highway-label_way_idx" ON "highway-label" USING gist (way);
CREATE INDEX
DROP TABLE IF EXISTS "park";
psql:/tmp/osmsld/create_tables.sql:78: NOTICE:  table "park" does not exist, skipping
DROP TABLE
CREATE TABLE "park" AS ( SELECT *, (CASE 
            WHEN way_area >= 10000000 THEN 'huge'
            WHEN way_area >= 1000000 THEN 'large'
            WHEN way_area >= 100000 THEN 'medium'
            ELSE 'small' END) AS size  FROM planet_osm_polygon
          WHERE "leisure" IN ('dog_park', 'golf_course', 'pitch', 'park',
            'playground', 'garden', 'common')
            OR "landuse" IN ('allotments', 'cemetery','recreation_ground', 'village_green')
          ORDER BY z_order asc );
SELECT 41058
CREATE INDEX "park_way_idx" ON "park" USING gist (way);
CREATE INDEX
DROP TABLE IF EXISTS "parking-area";
psql:/tmp/osmsld/create_tables.sql:90: NOTICE:  table "parking-area" does not exist, skipping
DROP TABLE
CREATE TABLE "parking-area" AS (
  SELECT way,amenity 
  FROM planet_osm_polygon
  WHERE amenity = 'parking'
);
SELECT 9322
CREATE INDEX "parking-area_way_idx" ON "parking-area" USING gist (way);
CREATE INDEX
DROP TABLE IF EXISTS "placenames-medium";
psql:/tmp/osmsld/create_tables.sql:98: NOTICE:  table "placenames-medium" does not exist, skipping
DROP TABLE
CREATE TABLE "placenames-medium" AS ( 
  SELECT way,place,name
  FROM planet_osm_point
  WHERE place IN ('city','metropolis','town','large_town','small_town')
);
SELECT 29737
CREATE INDEX "placenames-medium_way_idx" ON "placenames-medium" USING gist (way);
CREATE INDEX
DROP TABLE IF EXISTS "route-bridge-0";
psql:/tmp/osmsld/create_tables.sql:106: NOTICE:  table "route-bridge-0" does not exist, skipping
DROP TABLE
CREATE TABLE "route-bridge-0" AS ( 
  SELECT way,highway,railway,aeroway,tunnel
  FROM planet_osm_line
  WHERE ( "highway" IS NOT NULL
  OR "railway" IS NOT NULL
  OR "aeroway" IN ('apron','runway','taxiway') )
  AND bridge IN ('yes','true','1','viaduct')
  AND (layer IS NULL OR layer = '0')
  ORDER BY z_order asc
 );
SELECT 41810
CREATE INDEX "route-bridge-0_way_idx" ON "route-bridge-0" USING gist (way);
CREATE INDEX
DROP TABLE IF EXISTS "route-bridge-1";
psql:/tmp/osmsld/create_tables.sql:119: NOTICE:  table "route-bridge-1" does not exist, skipping
DROP TABLE
CREATE TABLE "route-bridge-1" AS ( 
  SELECT way,highway,railway,aeroway,tunnel
  FROM planet_osm_line
  WHERE ( "highway" IS NOT NULL
  OR "railway" IS NOT NULL
  OR "aeroway" IN ('apron','runway','taxiway') )
  AND bridge IN ('yes','true','1','viaduct')
  AND layer = '1'
  ORDER BY z_order asc
 );
SELECT 255072
CREATE INDEX "route-bridge-1_way_idx" ON "route-bridge-1" USING gist (way);
CREATE INDEX
DROP TABLE IF EXISTS "route-bridge-2";
psql:/tmp/osmsld/create_tables.sql:132: NOTICE:  table "route-bridge-2" does not exist, skipping
DROP TABLE
CREATE TABLE "route-bridge-2" AS ( 
  SELECT way,highway,railway,aeroway,tunnel
  FROM planet_osm_line
  WHERE ( "highway" IS NOT NULL
  OR "railway" IS NOT NULL
  OR "aeroway" IN ('apron','runway','taxiway') )
  AND bridge IN ('yes','true','1','viaduct')
  AND layer = '2'
  ORDER BY z_order asc
 );
SELECT 9227
CREATE INDEX "route-bridge-2_way_idx" ON "route-bridge-2" USING gist (way);
CREATE INDEX
DROP TABLE IF EXISTS "route-bridge-3";
psql:/tmp/osmsld/create_tables.sql:145: NOTICE:  table "route-bridge-3" does not exist, skipping
DROP TABLE
CREATE TABLE "route-bridge-3" AS ( 
  SELECT way,highway,railway,aeroway,tunnel
  FROM planet_osm_line
  WHERE ( "highway" IS NOT NULL
  OR "railway" IS NOT NULL
  OR "aeroway" IN ('apron','runway','taxiway') )
  AND bridge IN ('yes','true','1','viaduct')
  AND layer = '3'
  ORDER BY z_order asc
 );
SELECT 1341
CREATE INDEX "route-bridge-3_way_idx" ON "route-bridge-3" USING gist (way);
CREATE INDEX
DROP TABLE IF EXISTS "route-bridge-4";
psql:/tmp/osmsld/create_tables.sql:158: NOTICE:  table "route-bridge-4" does not exist, skipping
DROP TABLE
CREATE TABLE "route-bridge-4" AS ( 
  SELECT way,highway,railway,aeroway,tunnel
  FROM planet_osm_line
  WHERE ( "highway" IS NOT NULL
  OR "railway" IS NOT NULL
  OR "aeroway" IN ('apron','runway','taxiway') )
  AND bridge IN ('yes','true','1','viaduct')
  AND layer = '4'
  ORDER BY z_order asc
 );
SELECT 278
CREATE INDEX "route-bridge-4_way_idx" ON "route-bridge-4" USING gist (way);
CREATE INDEX
DROP TABLE IF EXISTS "route-bridge-5";
psql:/tmp/osmsld/create_tables.sql:171: NOTICE:  table "route-bridge-5" does not exist, skipping
DROP TABLE
CREATE TABLE "route-bridge-5" AS ( 
  SELECT way,highway,railway,aeroway,tunnel
  FROM planet_osm_line
  WHERE ( "highway" IS NOT NULL
  OR "railway" IS NOT NULL
  OR "aeroway" IN ('apron','runway','taxiway') )
  AND bridge IN ('yes','true','1','viaduct')
  AND layer = '5'
  ORDER BY z_order asc
 );
SELECT 165
CREATE INDEX "route-bridge-5_way_idx" ON "route-bridge-5" USING gist (way);
CREATE INDEX
DROP TABLE IF EXISTS "route-fill";
psql:/tmp/osmsld/create_tables.sql:184: NOTICE:  table "route-fill" does not exist, skipping
DROP TABLE
CREATE TABLE "route-fill" AS ( 
  SELECT way, highway, horse, bicycle, foot, 
    aeroway,
    case when tunnel in ('yes', 'true', '1')
      then 'yes'::text
      else tunnel end as tunnel,
    case when bridge in ('yes','true','1','viaduct')
      then 'yes'::text else bridge end as bridge,
    case when railway in ('spur','siding')
      or (railway='rail' and service in ('spur','siding','yard'))
      then 'spur-siding-yard'::text else railway
      end as railway,
    case when service in 
      ('parking_aisle', 'drive-through', 'driveway')
      then 'INT-minor'::text else service 
      end as service
  FROM planet_osm_line
  WHERE highway IS NOT NULL
    OR aeroway IN ( 'runway','taxiway' )
    OR railway IN ( 'light_rail', 'narrow_gauge', 'funicular',
      'rail', 'subway', 'tram', 'spur', 'siding', 'platform',
      'disused', 'abandoned', 'construction', 'miniature' )
  ORDER BY z_order);
SELECT 2308543
CREATE INDEX "route-fill_way_idx" ON "route-fill" USING gist (way);
CREATE INDEX
DROP TABLE IF EXISTS "route-line";
psql:/tmp/osmsld/create_tables.sql:210: NOTICE:  table "route-line" does not exist, skipping
DROP TABLE
CREATE TABLE "route-line" AS ( 
  SELECT way,highway,aeroway,
    case when tunnel IN ( 'yes', 'true', '1' ) then 'yes'::text
      else 'no'::text end as tunnel,
    case when service IN ( 'parking_aisle',
      'drive-through','driveway' ) then 'INT-minor'::text
      else service end as service
  FROM planet_osm_line
  WHERE highway in ( 'motorway', 'motorway_link',
    'trunk', 'trunk_link', 'primary', 'primary_link',
    'secondary', 'secondary_link', 'tertiary', 'tertiary_link', 
    'residential', 'unclassified', 'road', 'service',
    'pedestrian', 'raceway', 'living_street' )
  OR "aeroway" IN ('apron','runway','taxiway')
  ORDER BY z_order);
SELECT 1964517
CREATE INDEX "route-line_way_idx" ON "route-line" USING gist (way);
CREATE INDEX
DROP TABLE IF EXISTS "route-tunnels";
psql:/tmp/osmsld/create_tables.sql:228: NOTICE:  table "route-tunnels" does not exist, skipping
DROP TABLE
CREATE TABLE "route-tunnels" AS ( 
  SELECT way,highway FROM planet_osm_line 
  WHERE highway IN
  ( 'motorway', 'motorway_link', 'trunk', 'trunk_link', 
    'primary', 'primary_link', 'secondary', 'secondary_link',
    'tertiary', 'tertiary_link', 'residential', 'unclassified' )
  AND tunnel IN ( 'yes', 'true', '1' )
  ORDER BY z_order );
SELECT 23129
CREATE INDEX "route-tunnels_way_idx" ON "route-tunnels" USING gist (way);
CREATE INDEX
DROP TABLE IF EXISTS "route-turning-circles";
psql:/tmp/osmsld/create_tables.sql:239: NOTICE:  table "route-turning-circles" does not exist, skipping
DROP TABLE
CREATE TABLE "route-turning-circles" AS ( SELECT highway,way FROM planet_osm_point
  WHERE "highway" = 'turning_circle' );
SELECT 1130
CREATE INDEX "route-turning-circles_way_idx" ON "route-turning-circles" USING gist (way);
CREATE INDEX
DROP TABLE IF EXISTS "water-outline";
psql:/tmp/osmsld/create_tables.sql:244: NOTICE:  table "water-outline" does not exist, skipping
DROP TABLE
CREATE TABLE "water-outline" AS ( 
  SELECT "natural", "landuse", "waterway", "way"
  FROM planet_osm_polygon
  WHERE "natural" IN ('lake','water')
  OR "waterway" IN ('canal','mill_pond','riverbank')
  OR "landuse" IN ('basin','reservoir','water')
  ORDER BY z_order ASC
);
SELECT 130859
CREATE INDEX "water-outline_way_idx" ON "water-outline" USING gist (way);
CREATE INDEX
DROP TABLE IF EXISTS "water";
psql:/tmp/osmsld/create_tables.sql:255: NOTICE:  table "water" does not exist, skipping
DROP TABLE
CREATE TABLE "water" AS ( 
  SELECT "natural", "landuse", "waterway", "way"
  FROM planet_osm_polygon
  WHERE "natural" IN ('lake','water')
  OR "waterway" IN ('canal','mill_pond','riverbank')
  OR "landuse" IN ('basin','reservoir','water')
  ORDER BY z_order asc
);
SELECT 130859
CREATE INDEX "water_way_idx" ON "water" USING gist (way);
CREATE INDEX
DROP TABLE IF EXISTS "wetland";
psql:/tmp/osmsld/create_tables.sql:266: NOTICE:  table "wetland" does not exist, skipping
DROP TABLE
CREATE TABLE "wetland" AS (
  SELECT way,"natural" FROM planet_osm_polygon
  WHERE "natural" IN ('marsh','wetland')
);
SELECT 1815
CREATE INDEX "wetland_way_idx" ON "wetland" USING gist (way);
CREATE INDEX
```


















```
[root@demo osmsld]# curl -v -u admin:geoserver -XPOST -d@layergroup.xml -H "Content-type: text/xml" http://localhost:8080/geoserver/rest/workspaces/chinaosm/layergroups
* About to connect() to localhost port 8080 (#0)
*   Trying ::1...
* Connected to localhost (::1) port 8080 (#0)
* Server auth using Basic with user 'admin'
> POST /geoserver/rest/workspaces/chinaosm/layergroups HTTP/1.1
> Authorization: Basic YWRtaW46Z2Vvc2VydmVy
> User-Agent: curl/7.29.0
> Host: localhost:8080
> Accept: */*
> Content-type: text/xml
> Content-Length: 755
> 
* upload completely sent off: 755 out of 755 bytes
< HTTP/1.1 201 Created
< Server: Apache-Coyote/1.1
< X-Frame-Options: SAMEORIGIN
< Location: http://localhost:8080/geoserver/rest/layergroups/osm
< Content-Type: application/json;charset=ISO-8859-1
< Content-Length: 3
< Date: Thu, 26 Apr 2018 15:42:16 GMT
< 
* Connection #0 to host localhost left intact
```









curl -v -u admin:geoserver -XPOST -d@layergroup.xml -H "Content-type: text/xml" http://localhost:8080/geoserver/rest/workspaces/chinaosm/layergroups















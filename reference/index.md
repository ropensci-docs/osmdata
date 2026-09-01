# Package index

## Package classes

- [`osmdata()`](https://docs.ropensci.org/osmdata/reference/osmdata.md)
  : osmdata class def

## Overpass server

- [`get_overpass_url()`](https://docs.ropensci.org/osmdata/reference/get_overpass_url.md)
  : get_overpass_url
- [`list_overpass_urls()`](https://docs.ropensci.org/osmdata/reference/list_overpass_urls.md)
  : list_overpass_urls
- [`set_overpass_url()`](https://docs.ropensci.org/osmdata/reference/set_overpass_url.md)
  : set_overpass_url

## Prepare queries

- [`add_osm_feature()`](https://docs.ropensci.org/osmdata/reference/add_osm_feature.md)
  : Add a feature to an Overpass query
- [`add_osm_features()`](https://docs.ropensci.org/osmdata/reference/add_osm_features.md)
  : Add multiple features to an Overpass query
- [`bbox_to_string()`](https://docs.ropensci.org/osmdata/reference/bbox_to_string.md)
  : Convert a named matrix or a named or unnamed vector or data.frame to
  a string
- [`filter_osm_user()`](https://docs.ropensci.org/osmdata/reference/filter_osm_user.md)
  : Add an user filter to an Overpass query
- [`getbb()`](https://docs.ropensci.org/osmdata/reference/getbb.md) :
  Get bounding box for a given place name
- [`opq()`](https://docs.ropensci.org/osmdata/reference/opq.md) : Build
  an Overpass query
- [`opq_around()`](https://docs.ropensci.org/osmdata/reference/opq_around.md)
  : opq_around
- [`opq_csv()`](https://docs.ropensci.org/osmdata/reference/opq_csv.md)
  : Transform an Overpass query to return the result in a csv format
- [`opq_enclosing()`](https://docs.ropensci.org/osmdata/reference/opq_enclosing.md)
  : opq_enclosing
- [`opq_osm_id()`](https://docs.ropensci.org/osmdata/reference/opq_osm_id.md)
  : Add a feature specified by OSM ID to an Overpass query
- [`opq_string()`](https://docs.ropensci.org/osmdata/reference/opq_string.md)
  : Convert an overpass query into a text string
- [`overpass_status()`](https://docs.ropensci.org/osmdata/reference/overpass_status.md)
  : Retrieve status of the Overpass API

## Get additional OSM info

- [`available_features()`](https://docs.ropensci.org/osmdata/reference/available_features.md)
  : List recognized features in OSM
- [`available_tags()`](https://docs.ropensci.org/osmdata/reference/available_tags.md)
  : List tags associated with a feature

## Extract data

- [`osmdata_data_frame()`](https://docs.ropensci.org/osmdata/reference/osmdata_data_frame.md)
  :

  Return an OSM Overpass query as a
  [data.frame](https://rdrr.io/r/base/data.frame.html) object.

- [`osmdata_sc()`](https://docs.ropensci.org/osmdata/reference/osmdata_sc.md)
  :

  Return an OSM Overpass query as an `osmdata_sc` object in `silicate`
  (`SC`) format.

- [`osmdata_sf()`](https://docs.ropensci.org/osmdata/reference/osmdata_sf.md)
  :

  Return an OSM Overpass query as an osmdata object in sf format.

- [`osmdata_sp()`](https://docs.ropensci.org/osmdata/reference/osmdata_sp.md)
  :

  DEPRECATED: Return an OSM Overpass query as an osmdata object in sp
  format.

- [`osmdata_xml()`](https://docs.ropensci.org/osmdata/reference/osmdata_xml.md)
  : Return an OSM Overpass query in XML format Read an (XML format) OSM
  Overpass response from a string, a connection, or a raw vector.

## Search data

- [`osm_lines()`](https://docs.ropensci.org/osmdata/reference/osm_lines.md)
  :

  Extract all `osm_lines` from an `osmdata_sf` object

- [`osm_multilines()`](https://docs.ropensci.org/osmdata/reference/osm_multilines.md)
  :

  Extract all `osm_multilines` from an `osmdata_sf` object

- [`osm_multipolygons()`](https://docs.ropensci.org/osmdata/reference/osm_multipolygons.md)
  :

  Extract all `osm_multipolygons` from an `osmdata_sf` object

- [`osm_points()`](https://docs.ropensci.org/osmdata/reference/osm_points.md)
  :

  Extract all `osm_points` from an `osmdata_sf` object

- [`osm_polygons()`](https://docs.ropensci.org/osmdata/reference/osm_polygons.md)
  :

  Extract all `osm_polygons` from an `osmdata_sf` object

## Transform data

- [`osm_elevation()`](https://docs.ropensci.org/osmdata/reference/osm_elevation.md)
  : osm_elevation
- [`osm_poly2line()`](https://docs.ropensci.org/osmdata/reference/osm_poly2line.md)
  : Convert osmdata polygons into lines
- [`trim_osmdata()`](https://docs.ropensci.org/osmdata/reference/trim_osmdata.md)
  : trim_osmdata
- [`unique_osmdata()`](https://docs.ropensci.org/osmdata/reference/unique_osmdata.md)
  : unique_osmdata
- [`unname_osmdata_sf()`](https://docs.ropensci.org/osmdata/reference/unname_osmdata_sf.md)
  : unname_osmdata_sf

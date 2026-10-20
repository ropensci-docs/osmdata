# Import OpenStreetMap data in 'sf', 'SC', 'sp', 'data.frame' and 'xml' formats

Imports OpenStreetMap (OSM) data into R as 'sf', 'SC', 'sp',
'data.frame' or 'xml_document' objects. OSM data are extracted from the
overpass API and processed with very fast C++ routines for return to R.
The package enables simple overpass queries to be constructed without
the user necessarily understanding the syntax of the overpass query
language, while retaining the ability to handle arbitrarily complex
queries. Functions are also provided to enable recursive searching
between different kinds of OSM data (for example, to find all lines
which intersect a given point).

## Functions to Prepare Queries

- [`getbb()`](https://docs.ropensci.org/osmdata/reference/getbb.md): Get
  bounding box for a given place name

- [`bbox_to_string()`](https://docs.ropensci.org/osmdata/reference/bbox_to_string.md):
  Convert a named matrix or a named vector (or an unnamed vector) return
  a string

- [`overpass_status()`](https://docs.ropensci.org/osmdata/reference/overpass_status.md):
  Retrieve status of the overpass API

- [`opq()`](https://docs.ropensci.org/osmdata/reference/opq.md): Build
  an overpass query

- [`add_osm_feature()`](https://docs.ropensci.org/osmdata/reference/add_osm_feature.md):
  Add a feature to an overpass query

- [`add_osm_features()`](https://docs.ropensci.org/osmdata/reference/add_osm_features.md):
  Add multiple features to an Overpass query

- [`filter_osm_user()`](https://docs.ropensci.org/osmdata/reference/filter_osm_user.md):
  Add an user filter to an Overpass query

- [`opq_string()`](https://docs.ropensci.org/osmdata/reference/opq_string.md):
  Convert an osmdata query to overpass API string

## Functions to Get Additional OSM Information

- [`available_features()`](https://docs.ropensci.org/osmdata/reference/available_features.md):
  List recognised features in OSM

- [`available_tags()`](https://docs.ropensci.org/osmdata/reference/available_tags.md):
  List tags associated with a feature

## Functions to Extract OSM Data

- [`osmdata_data_frame()`](https://docs.ropensci.org/osmdata/reference/osmdata_data_frame.md):
  Return OSM data in
  [`data.frame`](https://rdrr.io/r/base/data.frame.html) format

- [`osmdata_sc()`](https://docs.ropensci.org/osmdata/reference/osmdata_sc.md):
  Return OSM data in silicate format

- [`osmdata_sf()`](https://docs.ropensci.org/osmdata/reference/osmdata_sf.md):
  Return OSM data in sf format

- [`osmdata_sp()`](https://docs.ropensci.org/osmdata/reference/osmdata_sp.md):
  Return OSM data in sp format (DEPRECATED)

- [`osmdata_xml()`](https://docs.ropensci.org/osmdata/reference/osmdata_xml.md):
  Return OSM data in xml2 format

## Functions to Search Data

- [`osm_points()`](https://docs.ropensci.org/osmdata/reference/osm_points.md):
  Extract all `osm_points` objects

- [`osm_lines()`](https://docs.ropensci.org/osmdata/reference/osm_lines.md):
  Extract all `osm_lines` objects

- [`osm_polygons()`](https://docs.ropensci.org/osmdata/reference/osm_polygons.md):
  Extract all `osm_polygons` objects

- [`osm_multilines()`](https://docs.ropensci.org/osmdata/reference/osm_multilines.md):
  Extract all `osm_multilines` objects

- [`osm_multipolygons()`](https://docs.ropensci.org/osmdata/reference/osm_multipolygons.md):
  Extract all `osm_multipolygons` objects

## See also

Useful links:

- <https://docs.ropensci.org/osmdata/>

- <https://github.com/ropensci/osmdata>

- Report bugs at <https://github.com/ropensci/osmdata/issues>

## Author

Joan Maspons, Mark Padgham, Bob Rudis, Robin Lovelace, Maëlle Salmon

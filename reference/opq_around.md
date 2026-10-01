# opq_around

Find all features around a given point, and optionally match specific
'key'-'value' pairs. This function is *not* intended to be combined with
[`add_osm_feature()`](https://docs.ropensci.org/osmdata/reference/add_osm_feature.md),
rather is only to be used in the sequence `opq_around()` -\>
[`osmdata_xml()`](https://docs.ropensci.org/osmdata/reference/osmdata_xml.md)
(or other extraction function). See examples for how to use.

## Usage

``` r
opq_around(lon, lat, radius = 15, key = NULL, value = NULL, timeout = 25)
```

## Arguments

- lon:

  Longitude of desired point

- lat:

  Latitude of desired point

- radius:

  Radius in metres around the point for which data should be extracted.
  Queries with large values for this parameter may fail.

- key:

  (Optional) OSM key of enclosing data

- value:

  (Optional) OSM value matching 'key' of enclosing data

- timeout:

  It may be necessary to increase this value for large queries, because
  the server may time out before all data are delivered.

## See also

Other queries:
[`add_osm_feature()`](https://docs.ropensci.org/osmdata/reference/add_osm_feature.md),
[`add_osm_features()`](https://docs.ropensci.org/osmdata/reference/add_osm_features.md),
[`bbox_to_string()`](https://docs.ropensci.org/osmdata/reference/bbox_to_string.md),
[`filter_osm_user()`](https://docs.ropensci.org/osmdata/reference/filter_osm_user.md),
[`getbb()`](https://docs.ropensci.org/osmdata/reference/getbb.md),
[`opq()`](https://docs.ropensci.org/osmdata/reference/opq.md),
[`opq_csv()`](https://docs.ropensci.org/osmdata/reference/opq_csv.md),
[`opq_enclosing()`](https://docs.ropensci.org/osmdata/reference/opq_enclosing.md),
[`opq_osm_id()`](https://docs.ropensci.org/osmdata/reference/opq_osm_id.md),
[`opq_string()`](https://docs.ropensci.org/osmdata/reference/opq_string.md),
[`overpass_status()`](https://docs.ropensci.org/osmdata/reference/overpass_status.md)

## Examples

``` r
if (FALSE) { # \dontrun{
# Get all benches ("amenity=bench") within 100m of a particular point
lat <- 53.94542
lon <- -2.52017
key <- "amenity"
value <- "bench"
radius <- 100
x <- opq_around (lon, lat, radius, key, value) |>
    osmdata_sf ()
} # }
```

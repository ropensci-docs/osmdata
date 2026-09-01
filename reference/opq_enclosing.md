# opq_enclosing

Find all features which enclose a given point, and optionally match
specific 'key'-'value' pairs. This function is *not* intended to be
combined with
[`add_osm_feature()`](https://docs.ropensci.org/osmdata/reference/add_osm_feature.md),
rather is only to be used in the sequence `opq_enclosing()` -\>
[`opq_string()`](https://docs.ropensci.org/osmdata/reference/opq_string.md)
-\>
[`osmdata_xml()`](https://docs.ropensci.org/osmdata/reference/osmdata_xml.md)
(or other extraction function). See examples for how to use.

## Usage

``` r
opq_enclosing(
  lon = NULL,
  lat = NULL,
  key = NULL,
  value = NULL,
  enclosing = "relation",
  timeout = 25
)
```

## Arguments

- lon:

  Longitude of desired point

- lat:

  Latitude of desired point

- key:

  (Optional) OSM key of enclosing data

- value:

  (Optional) OSM value matching 'key' of enclosing data

- enclosing:

  Either 'relation' or 'way' for whether to return enclosing objects of
  those respective types (where generally 'relation' will correspond to
  multipolygon objects, and 'way' to polygon objects).

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
[`opq_around()`](https://docs.ropensci.org/osmdata/reference/opq_around.md),
[`opq_csv()`](https://docs.ropensci.org/osmdata/reference/opq_csv.md),
[`opq_osm_id()`](https://docs.ropensci.org/osmdata/reference/opq_osm_id.md),
[`opq_string()`](https://docs.ropensci.org/osmdata/reference/opq_string.md),
[`overpass_status()`](https://docs.ropensci.org/osmdata/reference/overpass_status.md)

## Examples

``` r
if (FALSE) { # \dontrun{
# Get water body surrounding a particular point:
lat <- 54.33601
lon <- -3.07677
key <- "natural"
value <- "water"
x <- opq_enclosing (lon, lat, key, value) |>
    opq_string () |>
    osmdata_sf ()
} # }
```

# Convert a named matrix or a named or unnamed vector or data.frame to a string

This function converts a bounding box into a string for use in web apis

## Usage

``` r
bbox_to_string(bbox)
```

## Arguments

- bbox:

  bounding box as character, matrix, vector or a data.frame with
  `osm_type` and `osm_id` columns. If character, the bbox will be found
  (geocoded) and extracted with
  [`getbb()`](https://docs.ropensci.org/osmdata/reference/getbb.md).
  Unnamed vectors will be sorted appropriately and must merely be in the
  order (x, y, x, y).

## Value

A character string representing min x, min y, max x, and max y bounds.
For example: `"15.3152361,76.4406446,15.3552361,76.4806446"` is the
bounding box for Hampi, India. For data.frames with OSM objects, a
character string representing a set of OSM objects in overpass query
language. For example: `"relation(id:11747082)"` represents the area of
the Catalan Countries. A set of objects can also be represented for
multirow data.frames (e.g.
`"relation(id:11747082,307833); way(id:22422490)"`).

## See also

Other queries:
[`add_osm_feature()`](https://docs.ropensci.org/osmdata/reference/add_osm_feature.md),
[`add_osm_features()`](https://docs.ropensci.org/osmdata/reference/add_osm_features.md),
[`filter_osm_user()`](https://docs.ropensci.org/osmdata/reference/filter_osm_user.md),
[`getbb()`](https://docs.ropensci.org/osmdata/reference/getbb.md),
[`opq()`](https://docs.ropensci.org/osmdata/reference/opq.md),
[`opq_around()`](https://docs.ropensci.org/osmdata/reference/opq_around.md),
[`opq_csv()`](https://docs.ropensci.org/osmdata/reference/opq_csv.md),
[`opq_enclosing()`](https://docs.ropensci.org/osmdata/reference/opq_enclosing.md),
[`opq_osm_id()`](https://docs.ropensci.org/osmdata/reference/opq_osm_id.md),
[`opq_string()`](https://docs.ropensci.org/osmdata/reference/opq_string.md),
[`overpass_status()`](https://docs.ropensci.org/osmdata/reference/overpass_status.md)

## Examples

``` r
# Note input is (lon, lat) = (x, y) but output as needed for 'Overpass' API
# is (lat, lon) = (y, x).
bb <- c (-0.4325512, 39.2784496, -0.2725205, 39.566609)
bbox_to_string (bb)
#> [1] "39.2784496,-0.4325512,39.566609,-0.2725205"
# This is equivalent to:
if (FALSE) { # \dontrun{
bbox_to_string (getbb ("València"))
bbox_to_string (getbb ("València", format_out = "data.frame"))
} # }
```

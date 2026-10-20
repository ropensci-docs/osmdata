# Extract all `osm_points` from an `osmdata_sf` object

Extract all `osm_points` from an `osmdata_sf` object

## Usage

``` r
osm_points(dat, id)
```

## Arguments

- dat:

  An object of class `osmdata_sf`

- id:

  OSM identification of one or more objects for which points are to be
  extracted

## Value

An sf Simple Features Collection of points

## See also

Other search:
[`osm_lines()`](https://docs.ropensci.org/osmdata/reference/osm_lines.md),
[`osm_multilines()`](https://docs.ropensci.org/osmdata/reference/osm_multilines.md),
[`osm_multipolygons()`](https://docs.ropensci.org/osmdata/reference/osm_multipolygons.md),
[`osm_polygons()`](https://docs.ropensci.org/osmdata/reference/osm_polygons.md)

## Examples

``` r
if (FALSE) { # \dontrun{
tr <- opq ("trentham australia") |> osmdata_sf ()
coliban <- tr$osm_lines [which (tr$osm_lines$name == "Coliban River"), ]
pts <- osm_points (tr, rownames (coliban)) # all points of river
# the waterfall point:
waterfall <- pts [which (pts$waterway == "waterfall"), ]
} # }
```

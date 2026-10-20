# Extract all `osm_multilines` from an `osmdata_sf` object

`id` must be of an `osm_points` or `osm_lines` object (and can not be
the `id` of an `osm_polygons` object because multilines by definition
contain no polygons. `osm_multilines` returns any multiline object(s)
which contain the object specified by `id`.

## Usage

``` r
osm_multilines(dat, id)
```

## Arguments

- dat:

  An object of class `osmdata_sf`

- id:

  OSM identification of one of more objects for which multilines are to
  be extracted

## Value

An sf Simple Features Collection of multilines

## See also

Other search:
[`osm_lines()`](https://docs.ropensci.org/osmdata/reference/osm_lines.md),
[`osm_multipolygons()`](https://docs.ropensci.org/osmdata/reference/osm_multipolygons.md),
[`osm_points()`](https://docs.ropensci.org/osmdata/reference/osm_points.md),
[`osm_polygons()`](https://docs.ropensci.org/osmdata/reference/osm_polygons.md)

## Examples

``` r
if (FALSE) { # \dontrun{
dat <- opq ("London UK") |>
    add_osm_feature (key = "name", value = "Thames", exact = FALSE) |>
    osmdata_sf ()
# Get ids of lines called "The Thames":
id <- rownames (dat$osm_lines [which (dat$osm_lines$name == "The Thames"), ])
# and find all multilinestring objects which include those lines:
osm_multilines (dat, id)
# Now note that
nrow (dat$osm_multilines) # = 24 multiline objects
nrow (osm_multilines (dat, id)) # = 1 - the recursive search selects the
# single multiline containing "The Thames"
} # }
```

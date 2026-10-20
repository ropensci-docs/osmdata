# Extract all `osm_lines` from an `osmdata_sf` object

If `id` is of a point object, `osm_lines` will return all lines
containing that point. If `id` is of a line or polygon object,
`osm_lines` will return all lines which intersect the given line or
polygon.

## Usage

``` r
osm_lines(dat, id)
```

## Arguments

- dat:

  An object of class `osmdata_sf`

- id:

  OSM identification of one or more objects for which lines are to be
  extracted

## Value

An sf Simple Features Collection of linestrings

## See also

Other search:
[`osm_multilines()`](https://docs.ropensci.org/osmdata/reference/osm_multilines.md),
[`osm_multipolygons()`](https://docs.ropensci.org/osmdata/reference/osm_multipolygons.md),
[`osm_points()`](https://docs.ropensci.org/osmdata/reference/osm_points.md),
[`osm_polygons()`](https://docs.ropensci.org/osmdata/reference/osm_polygons.md)

## Examples

``` r
if (FALSE) { # \dontrun{
dat <- opq ("hengelo nl") |>
    add_osm_feature (key = "highway") |>
    osmdata_sf ()
bus <- dat$osm_points [which (dat$osm_points$highway == "bus_stop"), ] |>
    rownames () # all OSM IDs of bus stops
osm_lines (dat, bus) # all highways containing bus stops

# All lines which intersect with Piccadilly Circus in London, UK
dat <- opq ("Fitzrovia London") |>
    add_osm_feature (key = "highway") |>
    osmdata_sf ()
i <- which (dat$osm_polygons$name == "Piccadilly Circus")
id <- rownames (dat$osm_polygons [i, ])
osm_lines (dat, id)
} # }
```

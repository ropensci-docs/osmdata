# Extract all `osm_polygons` from an `osmdata_sf` object

If `id` is of a point object, `osm_polygons` will return all polygons
containing that point. If `id` is of a line or polygon object,
`osm_polygons` will return all polygons which intersect the given line
or polygon.

## Usage

``` r
osm_polygons(dat, id)
```

## Arguments

- dat:

  An object of class `osmdata_sf`

- id:

  OSM identification of one or more objects for which polygons are to be
  extracted

## Value

An sf Simple Features Collection of polygons

## See also

Other search:
[`osm_lines()`](https://docs.ropensci.org/osmdata/reference/osm_lines.md),
[`osm_multilines()`](https://docs.ropensci.org/osmdata/reference/osm_multilines.md),
[`osm_multipolygons()`](https://docs.ropensci.org/osmdata/reference/osm_multipolygons.md),
[`osm_points()`](https://docs.ropensci.org/osmdata/reference/osm_points.md)

## Examples

``` r
# Extract polygons which intersect Conway Street in London
if (FALSE) { # \dontrun{
dat <- opq ("Marylebone London") |>
    add_osm_feature (key = "highway") |>
    osmdata_sf ()
conway <- which (dat$osm_lines$name == "Conway Street")
id <- rownames (dat$osm_lines [conway, ])
osm_polygons (dat, id)
} # }
```

# Extract all `osm_multipolygons` from an `osmdata_sf` object

`id` must be of an `osm_points`, `osm_lines`, or `osm_polygons` object.
`osm_multipolygons` returns any multipolygon object(s) which contain the
object specified by `id`.

## Usage

``` r
osm_multipolygons(dat, id)
```

## Arguments

- dat:

  An object of class `osmdata_sf`

- id:

  OSM identification of one or more objects for which multipolygons are
  to be extracted

## Value

An sf Simple Features Collection of multipolygons

## See also

Other search:
[`osm_lines()`](https://docs.ropensci.org/osmdata/reference/osm_lines.md),
[`osm_multilines()`](https://docs.ropensci.org/osmdata/reference/osm_multilines.md),
[`osm_points()`](https://docs.ropensci.org/osmdata/reference/osm_points.md),
[`osm_polygons()`](https://docs.ropensci.org/osmdata/reference/osm_polygons.md)

## Examples

``` r
# find all multipolygons which contain the single polygon called
# "Chiswick Eyot" (which is an island).
if (FALSE) { # \dontrun{
dat <- opq ("London UK") |>
    add_osm_feature (key = "name", value = "Thames", exact = FALSE) |>
    osmdata_sf ()
index <- which (dat$osm_multipolygons$name == "Chiswick Eyot")
id <- rownames (dat$osm_polygons [id, ])
osm_multipolygons (dat, id)
# That multipolygon is the Thames itself, but note that
nrow (dat$osm_multipolygons) # = 14 multipolygon objects
nrow (osm_multipolygons (dat, id)) # = 1 - the main Thames multipolygon
} # }
```

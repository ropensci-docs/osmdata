# trim_osmdata

Trim an `osmdata` object to within a bounding polygon

## Usage

``` r
trim_osmdata(dat, bb_poly, exclude = TRUE)
```

## Arguments

- dat:

  An `osmdata` object returned from
  [`osmdata_sf()`](https://docs.ropensci.org/osmdata/reference/osmdata_sf.md)
  or
  [`osmdata_sc()`](https://docs.ropensci.org/osmdata/reference/osmdata_sc.md).

- bb_poly:

  An `sf` or `sfc` object, or matrix representing a bounding polygon.
  Can be obtained with `getbb (..., format_out = "polygon")` or
  `getbb (..., format_out = "sf_polygon")` (and possibly selected from
  resultant list where multiple polygons are returned).

- exclude:

  If `TRUE`, objects are trimmed exclusively, only retaining those
  strictly within the bounding polygon; otherwise all objects which
  partly extend within the bounding polygon are retained.

## Value

A trimmed version of `dat`, reduced only to those components lying
within the bounding polygon.

## Note

It will generally be necessary to pre-load the sf package for this
function to work correctly.

Caution is advised when using polygons obtained from Nominatim via
`getbb(..., format_out = "polygon"|"sf_polygon")`. These shapes can be
outdated and thus could cause the trimming operation to not give results
expected based on the current state of the OSM data.

To reduce the downloaded data from Overpass, you can do the trimming in
the server-side using `getbb(..., format_out = "osm_type_id")` (see
examples).

## See also

Other transform:
[`osm_elevation()`](https://docs.ropensci.org/osmdata/reference/osm_elevation.md),
[`osm_poly2line()`](https://docs.ropensci.org/osmdata/reference/osm_poly2line.md),
[`unique_osmdata()`](https://docs.ropensci.org/osmdata/reference/unique_osmdata.md),
[`unname_osmdata_sf()`](https://docs.ropensci.org/osmdata/reference/unname_osmdata_sf.md)

## Examples

``` r
if (FALSE) { # \dontrun{
bb <- getbb ("colchester uk")
query <- opq (bb) |>
    add_osm_feature (key = "highway")
# Then extract data from 'Overpass' API
dat <- osmdata_sf (query, quiet = FALSE)
# Then get bounding *polygon* for Colchester, as opposed to rectangular
# bounding box, and use that to trim data within that polygon:
bb_pol <- getbb ("colchester uk", format_out = "polygon")
library (sf) # required for this function to work
dat_tr <- trim_osmdata (dat, bb_pol)
bb_sf <- getbb ("colchester uk", format_out = "sf_polygon")
class (bb_sf) # sf data.frame
dat_tr <- trim_osmdata (dat, bb_sf)
bb_sp <- as (bb_sf, "Spatial")
class (bb_sp) # SpatialPolygonsDataFrame
dat_tr <- trim_osmdata (dat, bb_sp)

# Server-side trimming equivalent
bb <- getbb ("colchester uk", format_out = "osm_type_id")
query <- opq (bb) |>
    add_osm_feature (key = "highway")
dat <- osmdata_sf (query, quiet = FALSE)
} # }
```

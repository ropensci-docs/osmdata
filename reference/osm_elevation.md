# osm_elevation

Add elevation data to a previously-extracted OSM data set, using a
pre-downloaded elevation file (see Details). Currently only works for
`SC`-class objects returned from
[`osmdata_sc()`](https://docs.ropensci.org/osmdata/reference/osmdata_sc.md).

## Usage

``` r
osm_elevation(dat, elev_file)
```

## Arguments

- dat:

  An `SC` object produced by
  [`osmdata_sc()`](https://docs.ropensci.org/osmdata/reference/osmdata_sc.md).

- elev_file:

  A vector of one or more character strings specifying paths to `.tif`
  files (or anything that terra can read) containing global elevation
  data. `.zip` files will be uncompressed.

## Value

A modified version of the input `dat` with an additional `z_` column
appended to the vertices.

## Details

Elevation data can be downloaded from
<https://portal.opentopography.org/raster?opentopoID=OTSRTM.082015.4326.1>,
<https://www.earthdata.nasa.gov/data/catalog/lpcloud-srtmgl1-003> or
similar.

## See also

Other transform:
[`osm_poly2line()`](https://docs.ropensci.org/osmdata/reference/osm_poly2line.md),
[`trim_osmdata()`](https://docs.ropensci.org/osmdata/reference/trim_osmdata.md),
[`unique_osmdata()`](https://docs.ropensci.org/osmdata/reference/unique_osmdata.md),
[`unname_osmdata_sf()`](https://docs.ropensci.org/osmdata/reference/unname_osmdata_sf.md)

## Examples

``` r
if (FALSE) { # \dontrun{
query <- opq ("omaha nebraska") |>
    add_osm_feature (key = "highway")
# Elevation can only be applied to \pkg{silicate} 'SC'-class data:
dat <- osmdata_sc (query)
dat$vertex
# The vertex table will have columns ("x_", "y_", "vertex_"). Then
# download elevation data, and add elevation column, "z_" with:
dat <- osm_elevation (dat, elev_file = "/path/to/elevation/data.tiff")
} # }
```

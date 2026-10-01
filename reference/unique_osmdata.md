# unique_osmdata

Reduce the components of an
[osmdata](https://docs.ropensci.org/osmdata/reference/osmdata.md) object
in 'sf' or 'sp' form (that is, obtained from
[`osmdata_sf()`](https://docs.ropensci.org/osmdata/reference/osmdata_sf.md)
or
[`osmdata_sp()`](https://docs.ropensci.org/osmdata/reference/osmdata_sp.md)
to only unique items of each type. That is, reduce `$osm_points` to only
those points not present in other objects (lines, polygons, etc.);
reduce `$osm_lines` to only those lines not present in multiline
objects; and reduce `$osm_polygons` to only those polygons not present
in multipolygon objects. This renders an
[osmdata](https://docs.ropensci.org/osmdata/reference/osmdata.md) object
more directly compatible with typical output of sf.

## Usage

``` r
unique_osmdata(dat)
```

## Arguments

- dat:

  An object of class `osmdata_sf` or `osmdata_sp`

## Value

Equivalent object reduced to only unique objects of each type

## See also

Other transform:
[`osm_elevation()`](https://docs.ropensci.org/osmdata/reference/osm_elevation.md),
[`osm_poly2line()`](https://docs.ropensci.org/osmdata/reference/osm_poly2line.md),
[`trim_osmdata()`](https://docs.ropensci.org/osmdata/reference/trim_osmdata.md),
[`unname_osmdata_sf()`](https://docs.ropensci.org/osmdata/reference/unname_osmdata_sf.md)

## Examples

``` r
if (FALSE) { # \dontrun{
query <- opq ("colchester uk") |>
    add_osm_feature (key = "highway")
# Then extract data from 'Overpass' API
dat <- osmdata_sf (query)
dat
# Then reduce to unique items of each type only:
dat <- unique_osmdata (dat)
dat
} # }
# And objects of each type (points, line, polygons, and so on) now have
# fewer members.
```

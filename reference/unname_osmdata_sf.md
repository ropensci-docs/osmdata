# unname_osmdata_sf

Remove names from `osmdata` geometry objects, for cases in which these
cause issues, particularly with plotting, such as
<https://github.com/rstudio/leaflet/issues/631>, or
<https://github.com/r-spatial/sf/issues/1177>. Note that removing these
names also removes any ability to inter-relate the different components
of an `osmdata` object, so use of this function is only recommended to
resolve issues such as those linked to above.

## Usage

``` r
unname_osmdata_sf(x)
```

## Arguments

- x:

  An 'osmdata_sf' object returned from function of same name

## Value

Same object, yet with no row names on geometry objects.

## See also

Other transform:
[`osm_elevation()`](https://docs.ropensci.org/osmdata/reference/osm_elevation.md),
[`osm_poly2line()`](https://docs.ropensci.org/osmdata/reference/osm_poly2line.md),
[`trim_osmdata()`](https://docs.ropensci.org/osmdata/reference/trim_osmdata.md),
[`unique_osmdata()`](https://docs.ropensci.org/osmdata/reference/unique_osmdata.md)

## Examples

``` r
if (FALSE) { # \dontrun{
query <- opq ("hampi india") |>
    add_osm_feature (key = "historic", value = "ruins")
# Then extract data from 'Overpass' API
hampi_sf <- osmdata_sf (query)
# Remove rownames from all geometry metrices:
hampi_clean <- unname_osmdata_sf (hampi_sf)

# All coordinate matrices include rownames with OSM ID values:
head (as.matrix (hampi_sf$osm_lines$geometry [[1]]))
# But 'unname_osmdata_sf' removes both row and column names:
head (as.matrix (hampi_clean$osm_lines$geometry [[1]]))
} # }
```

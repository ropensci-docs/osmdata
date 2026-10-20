# Convert osmdata polygons into lines

Street networks downloaded with `add_osm_object(key = "highway")` will
store any circular highways in `osm_polygons`. this function combines
those with the `osm_lines` component to yield a single sf `data.frame`
of all highways, whether polygonal or not.

## Usage

``` r
osm_poly2line(osmdat)
```

## Arguments

- osmdat:

  An `osmdata_sf` object.

## Value

Modified version of same object with all `osm_polygons` objects merged
into `osm_lines`.

## Note

The `osm_polygons` field is retained, with those features also repeated
as `LINESTRING` objects in `osm_lines`.

## See also

Other transform:
[`osm_elevation()`](https://docs.ropensci.org/osmdata/reference/osm_elevation.md),
[`trim_osmdata()`](https://docs.ropensci.org/osmdata/reference/trim_osmdata.md),
[`unique_osmdata()`](https://docs.ropensci.org/osmdata/reference/unique_osmdata.md),
[`unname_osmdata_sf()`](https://docs.ropensci.org/osmdata/reference/unname_osmdata_sf.md)

## Examples

``` r
if (FALSE) { # \dontrun{
query <- opq ("colchester uk") |>
    add_osm_feature (key = "highway")
# Then extract data from 'Overpass' API
dat <- osmdata_sf (query)
# colchester has lots of roundabouts, and these are stored in 'osm_polygons'
# rather than 'osm_lines'. The former can be merged with the latter by:
dat2 <- osm_poly2line (dat)
} # }
# 'dat2' will have more lines than 'dat', but the same number of polygons
# (they are left unchanged.)
```

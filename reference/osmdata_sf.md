# Return an OSM Overpass query as an [osmdata](https://docs.ropensci.org/osmdata/reference/osmdata.md) object in sf format.

Return an OSM Overpass query as an
[osmdata](https://docs.ropensci.org/osmdata/reference/osmdata.md) object
in sf format.

## Usage

``` r
osmdata_sf(q, doc, quiet = TRUE, stringsAsFactors = FALSE)
```

## Arguments

- q:

  An object of class `overpass_query` constructed with
  [`opq()`](https://docs.ropensci.org/osmdata/reference/opq.md) and
  [`add_osm_feature()`](https://docs.ropensci.org/osmdata/reference/add_osm_feature.md)
  or a string with a valid query, such as
  `"(node(39.4712701,-0.3841326,39.4713799,-0.3839475);); out;"`. May be
  be omitted, in which case the
  [osmdata](https://docs.ropensci.org/osmdata/reference/osmdata.md)
  object will not include the query. See examples below.

- doc:

  If missing, `doc` is obtained by issuing the overpass query, `q`,
  otherwise either the name of a file from which to read data, or an
  object of class xml2 returned from
  [`osmdata_xml()`](https://docs.ropensci.org/osmdata/reference/osmdata_xml.md).

- quiet:

  suppress status messages.

- stringsAsFactors:

  Should character strings in 'sf' 'data.frame' be coerced to factors?

## Value

An object of class `osmdata_sf` with the OSM components (points, lines,
and polygons) represented in sf format.

## Note

In 'dplyr'-type workflows in which the output of this function is piped
to other functions, it will generally be necessary to explicitly load
the sf package into the current workspace with 'library(sf)'.

## See also

Other extract:
[`osmdata_data_frame()`](https://docs.ropensci.org/osmdata/reference/osmdata_data_frame.md),
[`osmdata_sc()`](https://docs.ropensci.org/osmdata/reference/osmdata_sc.md),
[`osmdata_sp()`](https://docs.ropensci.org/osmdata/reference/osmdata_sp.md),
[`osmdata_xml()`](https://docs.ropensci.org/osmdata/reference/osmdata_xml.md)

## Examples

``` r
if (FALSE) { # \dontrun{
query <- opq ("hampi india") |>
    add_osm_feature (key = "historic", value = "ruins")
# Then extract data from 'Overpass' API
hampi_sf <- osmdata_sf (query)
} # }

# Complex query as a string (not possible with regular osmdata functions)
q <- '[out:xml][timeout:50];
    area[name="Països Catalans"][boundary=political]->.boundaryarea;

    rel(area.boundaryarea)[admin_level=8][boundary=administrative];
    map_to_area -> .all_level_8_areas;

    ( nwr(area.boundaryarea)[amenity=townhall]; >; );
    is_in;
    area._[admin_level=8][boundary=administrative] -> .level_8_areas_with_townhall;

    (.all_level_8_areas; - .level_8_areas_with_townhall;);
    rel(pivot);
    (._; >;);
    out;'

if (FALSE) { # \dontrun{
no_townhall <- osmdata_sf (q)
no_townhall
} # }
```

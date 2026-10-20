# Return an OSM Overpass query as a [data.frame](https://rdrr.io/r/base/data.frame.html) object.

Return an OSM Overpass query as a
[data.frame](https://rdrr.io/r/base/data.frame.html) object.

## Usage

``` r
osmdata_data_frame(q, doc, quiet = TRUE, stringsAsFactors = FALSE)
```

## Arguments

- q:

  An object of class `overpass_query` constructed with
  [`opq()`](https://docs.ropensci.org/osmdata/reference/opq.md) and
  [`add_osm_feature()`](https://docs.ropensci.org/osmdata/reference/add_osm_feature.md)
  or a string with a valid query, such as
  `"(node(39.4712701,-0.3841326,39.4713799,-0.3839475);); out;"`. May be
  be omitted, in which case the attributes of the
  [data.frame](https://rdrr.io/r/base/data.frame.html) will not include
  the query. See examples below.

- doc:

  If missing, `doc` is obtained by issuing the overpass query, `q`,
  otherwise either the name of a file from which to read data, or an
  object of class xml2 returned from
  [`osmdata_xml()`](https://docs.ropensci.org/osmdata/reference/osmdata_xml.md).

- quiet:

  suppress status messages.

- stringsAsFactors:

  Should character strings in the 'data.frame' be coerced to factors?

## Value

A `data.frame` inheriting from `osmdata_data.frame` class with id, type
and tags of the the objects from the query.

## Details

If you are not interested in the geometries of the results, it's a good
option to query for objects that match the features only and forget
about members of the ways and relations. You can achieve this by passing
the parameter `body = "tags"` to
[`opq()`](https://docs.ropensci.org/osmdata/reference/opq.md).

## See also

Other extract:
[`osmdata_sc()`](https://docs.ropensci.org/osmdata/reference/osmdata_sc.md),
[`osmdata_sf()`](https://docs.ropensci.org/osmdata/reference/osmdata_sf.md),
[`osmdata_sp()`](https://docs.ropensci.org/osmdata/reference/osmdata_sp.md),
[`osmdata_xml()`](https://docs.ropensci.org/osmdata/reference/osmdata_xml.md)

## Examples

``` r
if (FALSE) { # \dontrun{
query <- opq ("hampi india") |>
    add_osm_feature (key = "historic", value = "ruins")
# Then extract data from 'Overpass' API
hampi_df <- osmdata_data_frame (query)
attr (hampi_df, "bbox")
attr (hampi_df, "overpass_call")
attr (hampi_df, "meta")
} # }

# Complex query as a string (not possible with regular osmdata functions)
q <- '[out:csv(::type, ::id, "name:ca", "wikidata")][timeout:50];
    area[name="Països Catalans"][boundary=political]->.boundaryarea;

    rel(area.boundaryarea)[admin_level=8][boundary=administrative];
    map_to_area -> .all_level_8_areas;

    ( nwr(area.boundaryarea)[amenity=townhall]; >; );
    is_in;
    area._[admin_level=8][boundary=administrative] -> .level_8_areas_with_townhall;

    (.all_level_8_areas; - .level_8_areas_with_townhall;);
    rel(pivot);
    out tags;'

if (FALSE) { # \dontrun{
no_townhall <- osmdata_data_frame (q)
no_townhall
} # }
```

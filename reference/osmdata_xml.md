# Return an OSM Overpass query in XML format Read an (XML format) OSM Overpass response from a string, a connection, or a raw vector.

Return an OSM Overpass query in XML format Read an (XML format) OSM
Overpass response from a string, a connection, or a raw vector.

## Usage

``` r
osmdata_xml(q, filename, quiet = TRUE, encoding)
```

## Arguments

- q:

  An object of class `overpass_query` constructed with
  [`opq()`](https://docs.ropensci.org/osmdata/reference/opq.md) and
  [`add_osm_feature()`](https://docs.ropensci.org/osmdata/reference/add_osm_feature.md)
  or a string with a valid query, such as
  `"(node(39.4712701,-0.3841326,39.4713799,-0.3839475);); out;"`. See
  examples below.

- filename:

  If given, OSM data are saved to the named file

- quiet:

  suppress status messages.

- encoding:

  Unless otherwise specified XML documents are assumed to be encoded as
  UTF-8 or UTF-16. If the document is not UTF-8/16, and lacks an
  explicit encoding directive, this allows you to supply a default.

## Value

An object of class `xml2::xml_document` containing the result of the
overpass API query.

## Note

Objects of class `xml_document` can be saved as `.xml` or `.osm` files
with [`xml2::write_xml`](http://xml2.r-lib.org/reference/write_xml.md).

## See also

Other extract:
[`osmdata_data_frame()`](https://docs.ropensci.org/osmdata/reference/osmdata_data_frame.md),
[`osmdata_sc()`](https://docs.ropensci.org/osmdata/reference/osmdata_sc.md),
[`osmdata_sf()`](https://docs.ropensci.org/osmdata/reference/osmdata_sf.md),
[`osmdata_sp()`](https://docs.ropensci.org/osmdata/reference/osmdata_sp.md)

## Examples

``` r
if (FALSE) { # \dontrun{
query <- opq ("hampi india") |>
    add_osm_feature (key = "historic", value = "ruins")
# Then extract data from 'Overpass' API and save to local file:
osmdata_xml (query, filename = "hampi.osm")
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
    out tags;'

if (FALSE) { # \dontrun{
no_townhall <- osmdata_xml (q)
no_townhall
} # }
```

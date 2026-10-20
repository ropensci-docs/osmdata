# Transform an Overpass query to return the result in a csv format

Transform an Overpass query to return the result in a csv format

## Usage

``` r
opq_csv(q, fields, header = TRUE)
```

## Arguments

- q:

  A opq string or an object of class `overpass_query` constructed with
  [`opq()`](https://docs.ropensci.org/osmdata/reference/opq.md) or
  alternative opq builders (+
  [`add_osm_feature()`](https://docs.ropensci.org/osmdata/reference/add_osm_feature.md)/s).

- fields:

  a character vector with the field names.

- header:

  if `FALSE`, do not ask for column names.

## Value

The `overpass_query` or string with the prefix changed to return a csv.

## Details

The output format `csv`, ask for results in csv. See [CSV output
mode](https://wiki.openstreetmap.org/wiki/Overpass_API/Overpass_QL#CSV_output_mode)
for details. To get the data, use
[`osmdata_data_frame()`](https://docs.ropensci.org/osmdata/reference/osmdata_data_frame.md).

## Note

csv queries that reach the timeout will return a 0 row data.frame
without any warning. Increase `timeout` in `q` if you don't see the
expected result.

## See also

Other queries:
[`add_osm_feature()`](https://docs.ropensci.org/osmdata/reference/add_osm_feature.md),
[`add_osm_features()`](https://docs.ropensci.org/osmdata/reference/add_osm_features.md),
[`bbox_to_string()`](https://docs.ropensci.org/osmdata/reference/bbox_to_string.md),
[`filter_osm_user()`](https://docs.ropensci.org/osmdata/reference/filter_osm_user.md),
[`getbb()`](https://docs.ropensci.org/osmdata/reference/getbb.md),
[`opq()`](https://docs.ropensci.org/osmdata/reference/opq.md),
[`opq_around()`](https://docs.ropensci.org/osmdata/reference/opq_around.md),
[`opq_enclosing()`](https://docs.ropensci.org/osmdata/reference/opq_enclosing.md),
[`opq_osm_id()`](https://docs.ropensci.org/osmdata/reference/opq_osm_id.md),
[`opq_string()`](https://docs.ropensci.org/osmdata/reference/opq_string.md),
[`overpass_status()`](https://docs.ropensci.org/osmdata/reference/overpass_status.md)

## Examples

``` r
if (FALSE) { # \dontrun{
q <- getbb ("Catalan Countries", format_out = "osm_type_id") |>
    opq (out = "tags center", osm_type = "relation", timeout = 100) |>
    add_osm_feature ("admin_level", "7") |>
    add_osm_feature ("boundary", "administrative") |>
    opq_csv (fields = c ("name", "::type", "::id", "::lat", "::lon"))
comarques <- osmdata_data_frame (q) # without timeout parameter, 0 rows

qid <- opq_osm_id (
    type = "relation",
    id = c ("341530", "1809102", "1664395", "343124"),
    out = "tags"
) |>
    opq_csv (fields = c ("name", "name:ca"))
cities <- osmdata_data_frame (qid)
} # }
```

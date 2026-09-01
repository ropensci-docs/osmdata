# Add an user filter to an Overpass query

Select objects last edited or ever touched by a specific user.

## Usage

``` r
filter_osm_user(opq, user, touched = FALSE, is_uid)
```

## Arguments

- opq:

  An `overpass_query` object.

- user:

  A vector of user names or user ids. If `is.numeric(user)` or
  `all(grepl(^[0-9]+$, user`)), assumes that `user` is a user id.

- touched:

  If `TRUE`, selects objects of which at least one version has been
  edited by `user`. If `FALSE` (default), selects objects with `user` as
  the last editor only.

- is_uid:

  If `FALSE`, assume that `user` are user names. Useful to filter for a
  user name which contains numbers only and otherwise would be
  considered an user id.

## Value

An [opq](https://docs.ropensci.org/osmdata/reference/opq.md) object.

## Details

The order of `filter_osm_user()` in a query construction is not relevant
and is applied to the whole query (i.e. it affects all the statements of
the query).

See
<https://dev.overpass-api.de/overpass-doc/en/criteria/misc_criteria.html#by_user>
for details.

## See also

Other queries:
[`add_osm_feature()`](https://docs.ropensci.org/osmdata/reference/add_osm_feature.md),
[`add_osm_features()`](https://docs.ropensci.org/osmdata/reference/add_osm_features.md),
[`bbox_to_string()`](https://docs.ropensci.org/osmdata/reference/bbox_to_string.md),
[`getbb()`](https://docs.ropensci.org/osmdata/reference/getbb.md),
[`opq()`](https://docs.ropensci.org/osmdata/reference/opq.md),
[`opq_around()`](https://docs.ropensci.org/osmdata/reference/opq_around.md),
[`opq_csv()`](https://docs.ropensci.org/osmdata/reference/opq_csv.md),
[`opq_enclosing()`](https://docs.ropensci.org/osmdata/reference/opq_enclosing.md),
[`opq_osm_id()`](https://docs.ropensci.org/osmdata/reference/opq_osm_id.md),
[`opq_string()`](https://docs.ropensci.org/osmdata/reference/opq_string.md),
[`overpass_status()`](https://docs.ropensci.org/osmdata/reference/overpass_status.md)

## Examples

``` r
# Notice the "::user" and "::uid" fields in the opq_csv()
q_csv <- opq (bbox = "relation(id:11755232)", out = "meta", osm_type = "node") |>
    filter_osm_user (user = "jmaspons") |>
    add_osm_feature (key = "name") |>
    opq_csv (c ("::type", "::id", "name", "name:ca", "::user", "::uid"))
cat (opq_string (q_csv))
#> [out:csv(::type, ::id, "name", "name:ca", ::user, ::uid)][timeout:25];
#> (relation(id:11755232); map_to_area->.searchArea; );
#> (
#>   node ["name"] (user:"jmaspons") (area.searchArea);
#> ); out meta;
# Warning: csv queries can fail without errors in long running queries. For timeouts,
# it returns empty results.
if (FALSE) { # \dontrun{
d_csv <- osmdata_data_frame (q_csv)
d_csv
} # }

q_touched <- opq (
    bbox = "relation(id:11755232)", out = "meta",
    osm_type = "node", timeout = 100
) |>
    filter_osm_user (user = "jmaspons", touched = TRUE) |>
    add_osm_feature (key = "name")
cat (opq_string (q_touched))
#> [out:xml][timeout:100];
#> (relation(id:11755232); map_to_area->.searchArea; );
#> (
#>   node ["name"] (user_touched:"jmaspons") (area.searchArea);
#> ); out meta;
if (FALSE) { # \dontrun{
d_touched <- osmdata_data_frame (q_touched)
d_touched
} # }
```

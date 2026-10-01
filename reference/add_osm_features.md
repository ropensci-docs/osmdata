# Add multiple features to an Overpass query

Alternative version of
[`add_osm_feature()`](https://docs.ropensci.org/osmdata/reference/add_osm_feature.md)
for creating single queries with multiple features. Key-value matching
may be controlled by using the filter symbols described in
<https://wiki.openstreetmap.org/wiki/Overpass_API/Overpass_QL#By_tag_.28has-kv.29>.

## Usage

``` r
add_osm_features(
  opq,
  features,
  bbox = NULL,
  key_exact = TRUE,
  value_exact = TRUE
)
```

## Arguments

- opq:

  An `overpass_query` object

- features:

  A named list or vector with the format `list("<key>" = "<value>")` or
  `c("<key>" = "<value>")` or a character vector of key-value pairs with
  keys and values enclosed in escape-formatted quotations. See examples
  for details.

- bbox:

  optional bounding box for the feature query; must be set if no opq
  query bbox has been set.

- key_exact:

  If FALSE, `key` is not interpreted exactly; see
  <https://wiki.openstreetmap.org/wiki/Overpass_API>

- value_exact:

  If FALSE, `value` is not interpreted exactly

## Value

An [opq](https://docs.ropensci.org/osmdata/reference/opq.md) object.

## `add_osm_feature` vs `add_osm_features`

Features defined within an `add_osm_features()` call are combined with a
logical OR.

Chained calls to either
[`add_osm_feature()`](https://docs.ropensci.org/osmdata/reference/add_osm_feature.md)
or `add_osm_features()` combines features from these calls in a logical
AND; this is analagous to chaining `dplyr::filter()` on a data frame.

`add_osm_features()` with only one feature is logically equivalent to
[`add_osm_feature()`](https://docs.ropensci.org/osmdata/reference/add_osm_feature.md).

## References

<https://wiki.openstreetmap.org/wiki/Map_Features>

## See also

Other queries:
[`add_osm_feature()`](https://docs.ropensci.org/osmdata/reference/add_osm_feature.md),
[`bbox_to_string()`](https://docs.ropensci.org/osmdata/reference/bbox_to_string.md),
[`filter_osm_user()`](https://docs.ropensci.org/osmdata/reference/filter_osm_user.md),
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
if (FALSE) { # \dontrun{
q <- opq ("portsmouth usa") |>
    add_osm_features (features = list (
        "amenity" = "restaurant",
        "amenity" = "pub"
    ))

q <- opq ("portsmouth usa") |>
    add_osm_features (features = c (
        "\"amenity\"=\"restaurant\"",
        "\"amenity\"=\"pub\""
    ))
cat (opq_string (q))
# This extracts in a single query the same result as the following:
q1 <- opq ("portsmouth usa") |>
    add_osm_feature (
        key = "amenity",
        value = "restaurant"
    )
q2 <- opq ("portsmouth usa") |>
    add_osm_feature (key = "amenity", value = "pub")
c (osmdata_sf (q1), osmdata_sf (q2)) # all restaurants OR pubs

# Get objects with keys (`natural` OR `waterway`) AND `name`
q_keys <- opq ("Badia del Vallès", osm_types = "nwr", out = "tags") |>
    add_osm_features (features = list (natural = NULL, waterway = NULL)) |>
    add_osm_feature (key = "name")
cat (opq_string (q_keys))
} # }
```

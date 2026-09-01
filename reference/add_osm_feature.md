# Add a feature to an Overpass query

Add a feature to an Overpass query

## Usage

``` r
add_osm_feature(
  opq,
  key,
  value,
  key_exact = TRUE,
  value_exact = TRUE,
  match_case = TRUE,
  bbox = NULL
)
```

## Arguments

- opq:

  An `overpass_query` object

- key:

  feature key; can be negated with an initial exclamation mark,
  `key = "!this"`, and can also be a vector if `value` is missing.

- value:

  value for feature key; can be negated with an initial exclamation
  mark, `value = "!this"`, and can also be a vector,
  `value = c ("this", "that")`.

- key_exact:

  If FALSE, `key` is not interpreted exactly; see
  <https://wiki.openstreetmap.org/wiki/Overpass_API>

- value_exact:

  If FALSE, `value` is not interpreted exactly

- match_case:

  If FALSE, matching for both `key` and `value` is not sensitive to case

- bbox:

  optional bounding box for the feature query; must be set if no opq
  query bbox has been set

## Value

An [opq](https://docs.ropensci.org/osmdata/reference/opq.md) object.

## Note

`key_exact` should generally be `TRUE`, because OSM uses a reasonably
well defined set of possible keys, as returned by
[`available_features()`](https://docs.ropensci.org/osmdata/reference/available_features.md).
Setting `key_exact = FALSE` allows matching of regular expressions on
OSM keys, as described in Section 6.1.5 of
<https://wiki.openstreetmap.org/wiki/Overpass_API/Overpass_QL>. The
actual query submitted to the overpass API can be obtained from
[`opq_string()`](https://docs.ropensci.org/osmdata/reference/opq_string.md).

## `add_osm_feature` vs `add_osm_features`

Features defined within an
[`add_osm_features()`](https://docs.ropensci.org/osmdata/reference/add_osm_features.md)
call are combined with a logical OR.

Chained calls to either `add_osm_feature()` or
[`add_osm_features()`](https://docs.ropensci.org/osmdata/reference/add_osm_features.md)
combines features from these calls in a logical AND; this is analagous
to chaining `dplyr::filter()` on a data frame.

[`add_osm_features()`](https://docs.ropensci.org/osmdata/reference/add_osm_features.md)
with only one feature is logically equivalent to `add_osm_feature()`.

## References

<https://wiki.openstreetmap.org/wiki/Map_Features>

## See also

Other queries:
[`add_osm_features()`](https://docs.ropensci.org/osmdata/reference/add_osm_features.md),
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
    add_osm_feature (
        key = "amenity",
        value = "restaurant"
    ) |>
    add_osm_feature (key = "amenity", value = "pub")
osmdata_sf (q) # all objects that are restaurants AND pubs (there are none!)
q1 <- opq ("portsmouth usa") |>
    add_osm_feature (
        key = "amenity",
        value = "restaurant"
    )
q2 <- opq ("portsmouth usa") |>
    add_osm_feature (key = "amenity", value = "pub")
c (osmdata_sf (q1), osmdata_sf (q2)) # all restaurants OR pubs

# Use of negation to extract all non-primary highways
q <- opq ("portsmouth uk") |>
    add_osm_feature (key = "highway", value = "!primary")

# key negation without warnings
q3 <- opq ("Vinçà", osm_type = "node") |>
    add_osm_feature (key = c ("name", "!name:ca"))
cat (opq_string (q3))
q4 <- opq ("el Carxe", osm_type = "node") |>
    add_osm_feature (key = "natural", value = "peak") |>
    add_osm_feature (key = "!ele")
cat (opq_string (q4))

# Get objects with keys (`natural` OR `waterway`) AND `name`
q_keys <- opq ("Badia del Vallès", osm_types = "nwr", out = "tags") |>
    add_osm_features (features = list (natural = NULL, waterway = NULL)) |>
    add_osm_feature (key = "name")
cat (opq_string (q_keys))
} # }
```

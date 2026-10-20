# Add a feature specified by OSM ID to an Overpass query

Add a feature specified by OSM ID to an Overpass query

## Usage

``` r
opq_osm_id(
  id = NULL,
  type = NULL,
  open_url = FALSE,
  out = "body",
  datetime = NULL,
  datetime2 = NULL,
  adiff = FALSE,
  timeout = 25,
  memsize
)
```

## Arguments

- id:

  One or more official OSM identifiers (long-form integers), which must
  be entered as either a character or *numeric* value (because R does
  not support long-form integers). id can also be a character string
  prefixed with the id type, e.g. "relation/11158003"

- type:

  Type of objects (recycled); must be either `node`, `way`, or
  `relation`. Optional if id is prefixed with the type.

- open_url:

  If `TRUE`, open the OSM page of the specified object in web browser.
  Multiple objects (`id` values) will be opened in multiple pages.

- out:

  The level of verbosity of the overpass result: `body` (geometries and
  tags, the default), `tags` (tags without geometry), `meta` (like
  body + Timestamp, Version, Changeset, User, User ID of the last
  edition), `skel` (geometries only), `tags center` (tags without
  geometry + the coordinates of the center of the bounding box) and
  `ids` (type and id of the objects only).

- datetime:

  If specified, a date and time to extract data from the OSM database as
  it was up to the specified date and time, as described at
  <https://wiki.openstreetmap.org/wiki/Overpass_API/Overpass_QL#date>.
  This *must* be in ISO8601 format ("YYYY-MM-DDThh:mm:ssZ"), where both
  the "T" and "Z" characters must be present.

- datetime2:

  If specified, return the *difference* in the OSM database between
  `datetime` and `datetime2`, where `datetime2 > datetime`. See
  <https://wiki.openstreetmap.org/wiki/Overpass_API/Overpass_QL#Difference_between_two_dates_(diff)>.

- adiff:

  If `TRUE`, query for [augmented
  difference](https://wiki.openstreetmap.org/wiki/Overpass_API/Overpass_QL#Augmented-difference_between_two_dates_(adiff)).
  The result indicates what happened to the modified and deleted OSM
  objects. Requires `datetime(2)*`.

- timeout:

  It may be necessary to increase this value for large queries, because
  the server may time out before all data are delivered.

- memsize:

  The default memory size for the 'overpass' server in *bytes*; may need
  to be increased in order to handle large queries.

## Value

An [opq](https://docs.ropensci.org/osmdata/reference/opq.md) object.

## Note

Extracting elements by ID requires explicitly specifying the type of
element.

## References

<https://wiki.openstreetmap.org/wiki/Overpass_API/Overpass_QL#By_element_id>

## See also

Other queries:
[`add_osm_feature()`](https://docs.ropensci.org/osmdata/reference/add_osm_feature.md),
[`add_osm_features()`](https://docs.ropensci.org/osmdata/reference/add_osm_features.md),
[`bbox_to_string()`](https://docs.ropensci.org/osmdata/reference/bbox_to_string.md),
[`filter_osm_user()`](https://docs.ropensci.org/osmdata/reference/filter_osm_user.md),
[`getbb()`](https://docs.ropensci.org/osmdata/reference/getbb.md),
[`opq()`](https://docs.ropensci.org/osmdata/reference/opq.md),
[`opq_around()`](https://docs.ropensci.org/osmdata/reference/opq_around.md),
[`opq_csv()`](https://docs.ropensci.org/osmdata/reference/opq_csv.md),
[`opq_enclosing()`](https://docs.ropensci.org/osmdata/reference/opq_enclosing.md),
[`opq_string()`](https://docs.ropensci.org/osmdata/reference/opq_string.md),
[`overpass_status()`](https://docs.ropensci.org/osmdata/reference/overpass_status.md)

## Examples

``` r
if (FALSE) { # \dontrun{
id <- c (1489221200, 1489221321, 1489221491)
dat1 <- opq_osm_id (type = "node", id = id) |>
    opq_string () |>
    osmdata_sf ()
dat1$osm_points # the desired nodes
id <- c (136190595, 136190596)
dat2 <- opq_osm_id (type = "way", id = id) |>
    opq_string () |>
    osmdata_sf ()
dat2$osm_lines # the desired ways
dat <- c (dat1, dat2) # The node and way data combined
# All in one (same result as dat)
id <- c (1489221200, 1489221321, 1489221491, 136190595, 136190596)
type <- c ("node", "node", "node", "way", "way")
datAiO <- opq_osm_id (id = id, type = type) |>
    opq_string () |>
    osmdata_sf ()
} # }
```

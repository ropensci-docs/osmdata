# Build an Overpass query

Build an Overpass query

## Usage

``` r
opq(
  bbox = NULL,
  nodes_only,
  osm_types = c("node", "way", "relation"),
  out = c("body", "tags", "meta", "skel", "tags center", "ids"),
  datetime = NULL,
  datetime2 = NULL,
  adiff = FALSE,
  timeout = 25,
  memsize
)
```

## Arguments

- bbox:

  Either (i) four numeric values specifying the maximal and minimal
  longitudes and latitudes, in the form `c(xmin, ymin, xmax, ymax)`
  or (ii) a character string in the form `xmin,ymin,xmax,ymax`. These
  will be passed to
  [`getbb()`](https://docs.ropensci.org/osmdata/reference/getbb.md) to
  be converted to a numerical bounding box. Can also be (iii) a matrix
  representing a bounding polygon as returned from
  `getbb(..., format_out = "polygon")`. To search in an area, (iv) a
  character string with a relation or a (closed) way id in the format
  `"way(id:1)"`, `"relation(id:1, 2)"` or
  `"relation(id:1, 2, 3); way(id:2)"` as returned by
  `getbb(..., format_out = "osm_type_id")` or
  [`bbox_to_string()`](https://docs.ropensci.org/osmdata/reference/bbox_to_string.md)
  with a `data.frame` from `getbb(..., format_out = "data.frame")` to
  select all areas combined (relations and ways).

- nodes_only:

  WARNING: this parameter is equivalent to `osm_types = "node"` and is
  DEPRECATED. If `TRUE`, query OSM nodes only.

- osm_types:

  A character vector with several OSM types to query: `node`, `way` and
  `relation` is the default. `nwr`, `nw`, `wr`, `nr` and `rel` are also
  valid types. Some OSM structures such as `place = "city"` or
  `highway = "traffic_signals"` are represented by nodes only. Queries
  are built by default to return all nodes, ways, and relation, but this
  can be very inefficient for node-only queries. Setting this value to
  `"node"` for such cases makes queries more efficient and, in
  [`osmdata_sf()`](https://docs.ropensci.org/osmdata/reference/osmdata_sf.md),
  the data will be returned in the `osm_points` list item only.

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

An `overpass_query` object

## Details

The `out` statement for `tags`, `tags center`and `id`, do not return
geometries. `adiff = TRUE` option is not implemented for all `osmdata_*`
functions yet. Use
[osmdata_xml](https://docs.ropensci.org/osmdata/reference/osmdata_xml.md)
or
[osmdata_data_frame](https://docs.ropensci.org/osmdata/reference/osmdata_data_frame.md)
to get the result of these queries. See the documentation of the [out
statement](https://wiki.openstreetmap.org/wiki/Overpass_API/Overpass_QL#out)
and [augmented
difference](https://wiki.openstreetmap.org/wiki/Overpass_API/Overpass_QL#Augmented-difference_between_two_dates_(adiff))
for more details about these options.

## Note

See
<https://wiki.openstreetmap.org/wiki/Overpass_API#Resource_management_options_.28osm-script.29>
for explanation of `timeout` and `memsize` (or `maxsize` in overpass
terms). Note in particular the comment that queries with arbitrarily
large `memsize` are likely to be rejected.

## See also

Other queries:
[`add_osm_feature()`](https://docs.ropensci.org/osmdata/reference/add_osm_feature.md),
[`add_osm_features()`](https://docs.ropensci.org/osmdata/reference/add_osm_features.md),
[`bbox_to_string()`](https://docs.ropensci.org/osmdata/reference/bbox_to_string.md),
[`filter_osm_user()`](https://docs.ropensci.org/osmdata/reference/filter_osm_user.md),
[`getbb()`](https://docs.ropensci.org/osmdata/reference/getbb.md),
[`opq_around()`](https://docs.ropensci.org/osmdata/reference/opq_around.md),
[`opq_csv()`](https://docs.ropensci.org/osmdata/reference/opq_csv.md),
[`opq_enclosing()`](https://docs.ropensci.org/osmdata/reference/opq_enclosing.md),
[`opq_osm_id()`](https://docs.ropensci.org/osmdata/reference/opq_osm_id.md),
[`opq_string()`](https://docs.ropensci.org/osmdata/reference/opq_string.md),
[`overpass_status()`](https://docs.ropensci.org/osmdata/reference/overpass_status.md)

## Examples

``` r
if (FALSE) { # \dontrun{
q <- getbb ("portsmouth", display_name_contains = "United States") |>
    opq () |>
    add_osm_feature ("amenity", "restaurant") |>
    add_osm_feature ("amenity", "pub")
osmdata_sf (q) # all objects that are restaurants AND pubs (there are none!)
q1 <- getbb ("portsmouth", display_name_contains = "United States") |>
    opq () |>
    add_osm_feature ("amenity", "restaurant")
q2 <- getbb ("portsmouth", display_name_contains = "United States") |>
    opq () |>
    add_osm_feature ("amenity", "pub")
c (osmdata_sf (q1), osmdata_sf (q2)) # all restaurants OR pubs

# Use `osm_types = "node"` to retrieve single point data only, such as for central
# locations of cities.
opq <- opq (bbox, osm_types = "node") |>
    add_osm_feature (key = "place", value = "city") |>
    osmdata_sf (quiet = FALSE)

# Filter by a search area
qa1 <- getbb ("Catalan Countries", format_out = "osm_type_id") |>
    opq (osm_types = "node") |>
    add_osm_feature (key = "capital", value = "4")
opqa1 <- osmdata_sf (qa1)
# Filter by a multiple search areas
bb <- getbb ("Vilafranca", format_out = "data.frame")
qa2 <- bbox_to_string (bb [bb$osm_type != "node", ]) |>
    opq (osm_types = "node") |>
    add_osm_feature (key = "place")
opqa2 <- osmdata_sf (qa2)
} # }
```

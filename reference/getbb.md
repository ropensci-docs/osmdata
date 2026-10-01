# Get bounding box for a given place name

This function uses the free Nominatim API provided by OpenStreetMap to
find the bounding box (bb) associated with place names.

## Usage

``` r
getbb(
  place_name,
  display_name_contains = NULL,
  viewbox = NULL,
  format_out = c("matrix", "data.frame", "string", "polygon", "sf_polygon",
    "osm_type_id"),
  base_url = "https://nominatim.openstreetmap.org",
  featuretype = "settlement",
  limit = 10,
  key = NULL,
  silent = TRUE
)
```

## Arguments

- place_name:

  The name of the place you're searching for or a wikidata id. For
  Wikidata, only `format_out`, `base_url` and `silent` are used; all
  other parameters are ignored.

- display_name_contains:

  Text string to match with display_name field returned by
  <https://wiki.openstreetmap.org/wiki/Nominatim>.

- viewbox:

  Focuses the search on the given area defined as a character string
  `"x1,y1,x2,y2"` or `c(x1, y1, x2, y2)`. Any two corner points of the
  box are accepted as long as they make a proper box. `x` is longitude,
  `y` is latitude.

- format_out:

  Character string indicating output format: `matrix` (default),
  `string` (see
  [`bbox_to_string()`](https://docs.ropensci.org/osmdata/reference/bbox_to_string.md)),
  `data.frame` (all 'hits' returned by Nominatim), `sf_polygon` (for
  polygons that work with the sf package), `polygon` (full polygonal
  bounding boxes for each match) or `osm_type_id` (string for quering
  inside deffined OSM areas
  [`bbox_to_string()`](https://docs.ropensci.org/osmdata/reference/bbox_to_string.md)).

- base_url:

  Base website from where data is queried.

- featuretype:

  The type of OSM feature (settlement is default; see Note).

- limit:

  How many results should the API return?

- key:

  The API key to use for services that require it.

- silent:

  Should the API be printed to screen? `TRUE` by default.

## Value

For `format_out = "matrix"`, the default, return the bounding box:

      min   max
    x ...   ...
    y ...   ...

If `format_out = "polygon"`, a list of polygons and multipolygons with
one item for each `nominatim` result. The items are named with the OSM
type and id. Each polygon is formed by one or more two-columns matrices
of polygonal longitude-latitude points. The first matrix represents the
outer boundary and the next ones represent holes. See examples below for
illustration.

If `format_out = "sf_polygon"`, a `sf` object. Each row correspond to a
`place_name` within `nominatim` result.

For `format_out = "osm_type_id"`, a character string representing an OSM
object in overpass query language. For example:
`"relation(id:11747082)"` represents the area of the Catalan Countries.
If one exact match exists with potentially multiple polygonal
boundaries, only the first relation or way is returned. A set of objects
can also be represented for multiple results (e.g.
`relation(id:11747082,307833); way(id:22422490)`). See examples below
for illustration. The OSM objects that can be used as [areas in overpass
queries](https://wiki.openstreetmap.org/wiki/Overpass_API/Overpass_QL#Map_way/relation_to_area_(map_to_area))
*must be closed rings* (ways or relations).

## Details

It was inspired by the functions `bbox` from the sp package, `bb` from
the tmaptools package and `bb_lookup` from the github package nominatim
package, which can be found at <https://github.com/hrbrmstr/nominatim>.

See <https://wiki.openstreetmap.org/wiki/Nominatim> for details.

## Note

Specific values of `featuretype` include "street", "city",
<https://wiki.openstreetmap.org/wiki/Nominatim> for details). The
default `featuretype = "settlement"` combines results from all
intermediate levels below "country" and above "streets". If the bounding
box or polygon of a city is desired, better results will usually be
obtained with `featuretype = "city"`.

## See also

Other queries:
[`add_osm_feature()`](https://docs.ropensci.org/osmdata/reference/add_osm_feature.md),
[`add_osm_features()`](https://docs.ropensci.org/osmdata/reference/add_osm_features.md),
[`bbox_to_string()`](https://docs.ropensci.org/osmdata/reference/bbox_to_string.md),
[`filter_osm_user()`](https://docs.ropensci.org/osmdata/reference/filter_osm_user.md),
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
getbb ("Salzburg")
# Select based on display_name, print query url
getbb ("Hereford", display_name_contains = "United States", silent = FALSE)
# top 3 matches as data frame
getbb ("Hereford", format_out = "data.frame", limit = 3)
getbb ("Hereford", format_out = "data.frame", viewbox = getbb ("England"))

# Examples of polygonal boundaries
bb <- getbb ("Milano, Italy", format_out = "polygon")
# A polygon and a multipolygon:
str (bb) # matrices of longitude/latitude pairs

bb_sf <- getbb ("kathmandu", format_out = "sf_polygon")
bb_sf
# sf:::plot.sf(bb_sf) # can be plotted if sf is installed
getbb ("london", format_out = "sf_polygon")

getbb ("València", format_out = "osm_type_id")
# Select multiple areas with format_out = "osm_type_id"
areas <- getbb ("València", format_out = "data.frame")
bbox_to_string (areas [areas$osm_type != "node", ])

# Search by wikidata id (València)
getbb ("Q5720", format_out = "data.frame")

# Using an alternative service (locationiq requires an API key)
# add LOCATIONIQ=type_your_api_key_here to .Renviron:
key <- Sys.getenv ("LOCATIONIQ")
if (nchar (key) == 32) {
    getbb (place_name,
        base_url = "https://locationiq.org/v1/search.php",
        key = key
    )
}
} # }
```

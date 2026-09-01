# 1. osmdata

## 1. Introduction

`osmdata` is an R package for downloading and using data from
OpenStreetMap ([OSM](https://www.openstreetmap.org/)). OSM is a global
open access mapping project, which is free and open under the [ODbL
licence](https://www.openstreetmap.org/copyright) (OpenStreetMap
contributors 2017). This has many benefits, ensuring transparent data
provenance and ownership, enabling real-time evolution of the database
and, by allowing anyone to contribute, encouraging democratic decision
making and citizen science (Johnson 2017). See the [OSM
wiki](https://wiki.openstreetmap.org/wiki/Contribute_map_data) to find
out how to contribute to the world’s open geographical data commons.

Unlike the
[`OpenStreetMap`](https://cran.r-project.org/package=OpenStreetMap)
package, which facilitates the download of raster tiles, `osmdata`
provides access to the vector data underlying OSM.

`osmdata` can be installed from CRAN with

``` r

install.packages ("osmdata")
```

and then loaded in the usual way:

``` r

library (osmdata)
```

    ## Data (c) OpenStreetMap contributors, ODbL 1.0. https://www.openstreetmap.org/copyright
    ## connected to: https://overpass-api.de/api/interpreter

The development version of `osmdata` can be installed with the `remotes`
package using the following command:

``` r

remotes::install_github ("ropensci/osmdata")
```

`osmdata` uses the [`overpass` API](https://overpass-api.de) to download

OpenStreetMap (OSM) data and can convert the results to a variety of
formats, including both Simple Features (typically of class `sf`) and
Spatial objects (e.g. `SpatialPointsDataFrame`), as defined by the
packages [`sf`](https://cran.r-project.org/package=sf) and
[`sp`](https://cran.r-project.org/package=sp) packages respectively.

`overpass` is a C++ library that serves OSM data over the web. All
`overpass` queries begin with a bounding box, defined in `osmdata` with
the function
[`opq()`](https://docs.ropensci.org/osmdata/reference/opq.md):

``` r

q <- opq (bbox = c (51.1, 0.1, 51.2, 0.2))
```

The following sub-section provides more detail on bounding boxes.
Following the initial
[`opq()`](https://docs.ropensci.org/osmdata/reference/opq.md) call,
`osmdata` queries are built by adding one or more ‘features’, which are
specified in terms of `key-value` pairs. For example, all paths, ways,
and roads are designated in OSM with `key=highway`, so that a query all
motorways in greater London (UK) can be constructed as follows:

``` r

q <- opq (bbox = "greater london uk") |>
    add_osm_feature (key = "highway", value = "motorway")
```

A detailed description of features is provided at the [OSM
wiki](https://wiki.openstreetmap.org/wiki/Map_Features), or the
`osmdata` function
[`available_features()`](https://docs.ropensci.org/osmdata/reference/available_features.md)
can be used to retrieve the comprehensive list of feature keys currently
used in OSM.

``` r

head (available_features ())
```

    ## [1] "4wd only"  "abandoned" "abutters"  "access"    "addr"      "addr:city"

There are two primary `osmdata` functions for obtaining data from a
query:
[`osmdata_sf()`](https://docs.ropensci.org/osmdata/reference/osmdata_sf.md)
and
[`osmdata_sp()`](https://docs.ropensci.org/osmdata/reference/osmdata_sp.md),
which return data in [Simple Features
(`sf`)](https://cran.r-project.org/package=sf) and [Spatial
(`sp`)](https://cran.r-project.org/package=sp) formats, respectively.
The typical workflow for extracting OSM data with `osmdata` thus
consists of the three lines:

``` r

x <- opq (bbox = "greater london uk") |>
    add_osm_feature (key = "highway", value = "motorway") |>
    osmdata_sf ()
```

The return object (`x`) is described in the third section below.

### 1.1 Bounding boxes: the `getbb()` function

While bounding boxes may be explicitly specified for the
[`opq()`](https://docs.ropensci.org/osmdata/reference/opq.md) function,
they are more commonly obtained from the
[`getbb()`](https://docs.ropensci.org/osmdata/reference/getbb.md)
function, which accepts character strings. As illustrated in the above
example, the
[`opq()`](https://docs.ropensci.org/osmdata/reference/opq.md) function
also accepts character strings, which are simply passed directly to
[`getbb()`](https://docs.ropensci.org/osmdata/reference/getbb.md) to
convert them to rectangular bounding boxes.

``` r

bb <- getbb ("Greater London, U.K.")
q <- opq (bbox = bb)
```

Note that the text string is not case sensitive, as illustrated in the
following code:

``` r

identical (q, opq (bbox = "greater london uk"))
## TRUE
```

Note also that
[`getbb()`](https://docs.ropensci.org/osmdata/reference/getbb.md) can
return a data frame reporting multiple matches or matrices representing
bounding polygons of matches:

``` r

bb_df <- getbb (place_name = "london", format_out = "data.frame")
bb_poly <- getbb (place_name = "london", format_out = "polygon")
```

The [`overpass API`](https://www.overpass-api.de) only accepts simple
rectangular bounding boxes, and so data requested with a bounding
polygon will actually be all data within the corresponding rectangular
bounding box, but such data may be subsequently trimmed to within the
polygon with the
[`trim_osmdata()`](https://docs.ropensci.org/osmdata/reference/trim_osmdata.md)
function, demonstrated in the code immediately below.

All highways from within the polygonal boundary of Greater London can be
extracted with,

``` r

bb <- getbb ("london uk", format_out = "polygon")
x <- opq (bbox = bb) |>
    add_osm_feature (key = "highway", value = "motorway") |>
    osmdata_sf () |>
    trim_osmdata (bb)
```

See `?trim_osmdata()` for further ways to obtain polygonally bounded
sets of OSM data.

The [`getbb()`](https://docs.ropensci.org/osmdata/reference/getbb.md)
function also allows specification of an explicit `featuretype`, such as
street, city, county, state, or country. The default value of
`settlement` combines all results below country and above streets. See
[`?getbb`](https://docs.ropensci.org/osmdata/reference/getbb.md) for
more details.

## 2. The overpass API

As mentioned, `osmdata` obtains OSM data from the
[`overpass API`](https://www.overpass-api.de), [which
is](https://wiki.openstreetmap.org/wiki/Overpass_API)

> a read-only API that serves up custom selected parts of the OSM map
> data.

The syntax of `overpass` queries is powerful yet hard to learn. This
section briefly introduces the structure of `overpass` queries in order
to help construct more efficient and powerful queries. Those wanting to
skip straight onto query construction in `osmdata` may safely jump ahead
to the [query example below](#query-example).

`osmdata` simplifies queries so that OSM data can be extracted with very
little understanding of the `overpass` query syntax, although it is
still possible to submit arbitrarily complex `overpass` queries via
`osmdata`. An excellent place to explore `overpass` queries specifically
and OSM data in general is the online interactive query builder at
[overpass-turbo](https://overpass-turbo.eu/), which includes a helpful
corrector function for incorrectly formatted queries. Examples of its
functionality in action can be found on the [OpenStreetMap
wiki](https://wiki.openstreetmap.org/wiki/Overpass_API/Overpass_API_by_Example),
with full details of the `overpass` query language given in the [Query
Language
Guide](https://wiki.openstreetmap.org/wiki/Overpass_API/Overpass_QL) as
well as the [overpass API Language
Guide](https://wiki.openstreetmap.org/wiki/Overpass_API/Language_Guide).

By default, `osmdata` sends queries to one of the four main [`overpass`
server
instances](https://wiki.openstreetmap.org/wiki/Overpass_API#Public_Overpass_API_instances),
such as `https://overpass-api.de/api/interpreter` but other servers
listed on the page linked to above can be used, thanks to functions that
*get* and *set* the base url:

``` r

get_overpass_url ()
```

    ## [1] "https://overpass-api.de/api/interpreter"

``` r

new_url <- "https://maps.mail.ru/osm/tools/overpass/api/interpreter"
```

``` r

set_overpass_url (new_url) # reset the base url (not run)
```

`osmdata` queries are lists of class `overpass_query`. The actual query
passed to the `overpass API` with a query can be obtained with the
function
[`opq_string()`](https://docs.ropensci.org/osmdata/reference/opq_string.md).
Applied to the preceding query, this function gives:

``` r

opq_string (q)
## [out:xml][timeout:25];
## (
##   node
##     ["highway"="motorway"]
##     (51.2867602,-0.510375,51.6918741,0.3340155);
##   way
##     ["highway"="motorway"]
##     (51.2867602,-0.510375,51.6918741,0.3340155);
##   relation
##     ["highway"="motorway"]
##     (51.2867602,-0.510375,51.6918741,0.3340155);
## );
## (._;>);out body;
```

The resultant output may be pasted directly into the
[overpass-turbo](https://overpass-turbo.eu/) online interactive query
builder. (The output of `opq_string` has been somewhat reformatted here
to reflect the format typically used in `overpass-turbo`.)

### 2.1. osmdata queries

As demonstrated above, an `osmdata` query begins by specifying a
bounding box with the function
[`opq()`](https://docs.ropensci.org/osmdata/reference/opq.md), followed
by specifying desired OSM features with
[`add_osm_feature()`](https://docs.ropensci.org/osmdata/reference/add_osm_feature.md).

``` r

q <- opq (bbox = "Kunming, China") |>
    add_osm_feature (key = "natural", value = "water")
```

This query will request all natural water water bodies in Kunming,
China. A particular water body may be requested through appending a
further call to
[`add_osm_feature()`](https://docs.ropensci.org/osmdata/reference/add_osm_feature.md):

``` r

q <- opq (bbox = "Kunming, China") |>
    add_osm_feature (key = "natural", value = "water") |>
    add_osm_feature (key = "name:en", value = "Dian", value_exact = FALSE)
```

Each successive call to
[`add_osm_feature()`](https://docs.ropensci.org/osmdata/reference/add_osm_feature.md)
**adds** features to a query. This query is thus a request for all
bodies of natural water **and** those with English names that include
‘Dian’. The requested data may be extracted through calling one of the
`osmdata_xml/sp/sf()` functions.

Single queries are always constructed through **adding** features, and
therefore correspond to logical **AND** operations: natural water bodies
**AND** those whose names include ‘Dian’. The equivalent **OR**
combination can be extracted with the [`add_osm_features()`
function](https://docs.ropensci.org/osmdata/reference/add_osm_features.html).
The following query represents the OR-equivalent of the above query,
requesting data on both all natural features with the value of `"water"`
OR all features whose English name is `"Dian"`.

``` r

q <- opq (bbox = "Kunming, China") |>
    add_osm_features (features = c (
        "\"natural\"=\"water\"",
        "\"name:en\"=\"Dian\""
    ))
```

Note that the `"="` symbols here requests features whose values exactly
match the given values. Other “filter” symbols are possible, as
described in the [overpass query language
definition](https://wiki.openstreetmap.org/wiki/Overpass_API/Overpass_QL#By_tag_.28has-kv.29),
including symbols for negation (`!=`), or approximate matching (`~`).

Passing this query to
[`osmdata_sf()`](https://docs.ropensci.org/osmdata/reference/osmdata_sf.html)
will return identical data to the following way to explicitly construct
an OR query through using the inbuilt `c` operator of `osmdata`.

``` r

dat1 <- opq (bbox = "Kunming, China") |>
    add_osm_feature (key = "natural", value = "water") |>
    osmdata_sf ()
dat2 <- opq (bbox = "Kunming, China") |>
    add_osm_feature (key = "name:en", value = "Dian", value_exact = FALSE) |>
    osmdata_sf ()
dat <- c (dat1, dat2)
```

While the “filter” symbols may be explicitly specified in [the
`add_osm_features()`
function](https://docs.ropensci.org/osmdata/reference/add_osm_features.html),
the single-feature version of [`add_osm_feature()`
function](https://docs.ropensci.org/osmdata/reference/add_osm_feature.html)
has several logical parameters to control matching without needing to
remember precise overpass syntax:

- `key_exact` can be set to `FALSE` to approximately match given keys;
- `value_exact` can be set to `FALSE` to approximately match given
  values; and
- `match_case` can be set to `FALSE` to match keys and values in both
  lower and upper case forms.

The previous query with `key = 'name:end'` and `value = 'Dian'` could
thus be replaced by the following:

``` r

add_osm_feature (
    key = "name", value = "dian",
    key_exact = FALSE,
    value_exact = FALSE,
    match_case = FALSE
)
```

### 2.2 Extracting `OSM` data from a query

The primary `osmdata` functions
[`osmdata_sf()`](https://docs.ropensci.org/osmdata/reference/osmdata_sf.md)
or
[`osmdata_sp()`](https://docs.ropensci.org/osmdata/reference/osmdata_sp.md)
pass these queries to `overpass` and return OSM data in corresponding
`sf` or `sp` format, respectively. Both of these functions also accept
direct `overpass` queries, such as those produced by the `osmdata`
function
[`opq_string()`](https://docs.ropensci.org/osmdata/reference/opq_string.md),
or copied directly from the [`overpass-turbo` query
builder](https://overpass-turbo.eu).

``` r

osmdata_sf (opq_string (q))
## Object of class 'osmdata' with:
##                  $bbox :
##         $overpass_call : The call submitted to the overpass API
##             $timestamp : [ Thurs 5 May 2017 14:33:54 ]
##            $osm_points : 'sf' Simple Features Collection with 360582 points
##            ...
```

Note that the result contains no value for `bbox`, because that
information is lost when the full `osmdata_query`, `q`, is converted to
a string. Nevertheless, the results of the two calls
`osmdata_sf (opq_string (q))` and `osmdata_sf (q)` differ only in the
values of `bbox` and `timestamp`, while returning otherwise identical
data.

In summary, `osmdata` queries are generally simplified versions of
potentially more complex `overpass` queries, although arbitrarily
complex `overpass` queries may be passed directly to the primary
`osmdata` functions. As illustrated above, `osmdata` queries are
generally constructed through initiating a query with
[`opq()`](https://docs.ropensci.org/osmdata/reference/opq.md), and then
specifying OSM features in terms of `key-value` pairs with
[`add_osm_feature()`](https://docs.ropensci.org/osmdata/reference/add_osm_feature.md),
along with judicious usage of the `key_exact`, `value_exact`, and
`match_case` parameters.

The simplest way to use `osmdata` is to simply request all data within a
given bounding box (warning - not intended to run):

``` r

q <- opq (bbox = "London City, U.K.")
lots_of_data <- osmdata_sf (q)
```

Queries are, however, usually more useful when refined through using
[`add_osm_feature()`](https://docs.ropensci.org/osmdata/reference/add_osm_feature.md),
which minimally requires a single `key` and returns all objects
specifying any value for that `key`:

``` r

not_so_much_data <- opq (bbox = "city of london uk") |>
    add_osm_feature (key = "highway") |>
    add_osm_feature (key = "name") |>
    osmdata_sf ()
```

`osmdata` will use that query to return all named highways within the
requested bounding box. Note that `key` specifications are requests for
features which must include those keys, yet most features will also
include many other keys, and thus `osmdata` objects generally list a
large number of distinct keys, as demonstrated below.

### 2.3. Query example

To appreciate query building in more concrete terms, let’s imagine that
we wanted to find all cycle paths in Seville, Spain:

``` r

q1 <- opq ("Sevilla") |>
    add_osm_feature (key = "highway", value = "cycleway")
cway_sev <- osmdata_sp (q1)
sp::plot (cway_sev$osm_lines)
```

![](https://cloud.githubusercontent.com/assets/1825120/23708182/fba4ed96-040c-11e7-90cd-3274d394030a.png)

Now imagine we want to make a more specific query that only extracts
designated cycleways or those which are bridges. Combining these into
one query will return only those that are designated cycleways **AND**
that are bridges:

``` r

des_bike <- osmdata_sf (q1)
q2 <- add_osm_feature (q1, key = "bridge", value = "yes")
des_bike_and_bridge <- osmdata_sf (q2)
nrow (des_bike_and_bridge$osm_points)
nrow (des_bike_and_bridge$osm_lines)
## [1] 99
## [1] 32
```

That query returns only 99 points and 32 lines. Designed cycleways
**OR** bridges can be obtained through simply combining multiple
`osmdata` objects with the `c` operator:

``` r

q2 <- opq ("Sevilla") |>
    add_osm_feature (key = "bridge", value = "yes")
bridge <- osmdata_sf (q2)
des_bike_or_bridge <- c (des_bike, bridge)
nrow (des_bike_or_bridge$osm_points)
nrow (des_bike_or_bridge$osm_lines)
## [1] 9757
## [1] 1061
```

And as expected, the `OR` operation produces more data than the
equivalent `AND`, showing the utility of combining `osmdata` objects
with the generic function [`c()`](https://rdrr.io/r/base/c.html).

## 3. The `osmdata` object

The `osmdata` extraction functions
([`osmdata_sf()`](https://docs.ropensci.org/osmdata/reference/osmdata_sf.md)
and
[`osmdata_sp()`](https://docs.ropensci.org/osmdata/reference/osmdata_sp.md)),
both return objects of class `osmdata`. The structure of `osmdata`
objects are clear from their default print method, illustrated using the
`bridge` example from the previous section:

``` r

bridge
##  Object of class 'osmdata' with:
##                   $bbox : 37.3002036,-6.0329182,37.4529579,-5.819157
##          $overpass_call : The call submitted to the overpass API
##              $timestamp : [ Thurs 5 May 2017 14:41:19 ]
##             $osm_points : 'sf' Simple Features Collection with 69 points
##              $osm_lines : 'sf' Simple Features Collection with 25 linestrings
##           $osm_polygons : 'sf' Simple Features Collection with 0 polygons
##         $osm_multilines : 'sf' Simple Features Collection with 0 multilinestrings
##      $osm_multipolygons : 'sf' Simple Features Collection with 0 multipolygons
```

As the results show, all `osmdata` objects should contain:

- A bounding box (which can be accessed with `bridge$bbox`)
- A time-stamp of the query (`bridge$timestamp`, useful for checking
  data is up-to-date)
- The spatial data, consisting of `osm_points`, `osm_lines`,
  `osm_polygons`, `osm_multilines` and `osm_multipolygons`.

Some or all of these can be empty: the example printed above contains
only points and lines. The more complex features of `osm_multilines` and
`osm_multipolygons` refer to OSM relations than contain multiple lines
and polygons.

The actual spatial data contained in an `osmdata` object are of either
`sp` format when extracted with
[`osmdata_sp()`](https://docs.ropensci.org/osmdata/reference/osmdata_sp.md)
or `sf` format when extracted with
[`osmdata_sf()`](https://docs.ropensci.org/osmdata/reference/osmdata_sf.md).

``` r

class (osmdata_sf (q)$osm_lines)
## [1] "sf"         "data.frame"
```

``` r

class (osmdata_sp (q)$osm_lines)
## [1] "SpatialLinesDataFrame"
## attr(,"package")
## [1] "sp"
```

In addition to these two functions, `osmdata` provides a third function,
[`osmdata_xml()`](https://docs.ropensci.org/osmdata/reference/osmdata_xml.md),
which allows raw OSM data to be returned and optionally saved to disk in
XML format. The following code demonstrates this function, beginning
with a new query.

``` r

dat <- opq (bbox = c (-0.12, 51.51, -0.11, 51.52)) |>
    add_osm_feature (key = "building") |>
    osmdata_xml (file = "buildings.osm")
class (dat)
## [1] "xml_document" "xml_node"
```

This call both returns the same data as the object `dat` and saves them
to the file `buildings.osm`. Downloaded XML data can be converted to
`sf` or `sp` formats by simply passing the data to the respective
`osmdata` functions, either as the name of a file or an XML object:

``` r

q <- opq (bbox = c (-0.12, 51.51, -0.11, 51.52)) |>
    add_osm_feature (key = "building")
doc <- osmdata_xml (q, "buildings.osm")
dat1 <- osmdata_sf (q, doc)
dat2 <- osmdata_sf (q, "buildings.osm")
identical (dat1, dat2)
## [1] TRUE
```

The following sub-sections now explore these three functions in more
detail, beginning with
[`osmdata_xml()`](https://docs.ropensci.org/osmdata/reference/osmdata_xml.md).

### 3.1. The `osmdata_xml()` function

[`osmdata_xml()`](https://docs.ropensci.org/osmdata/reference/osmdata_xml.md)
returns OSM data in native XML format, and also allows these data to be
saved directly to disk (conventionally using the file suffix `.osm`,
although any suffix may be used). The `XML` data are formatting using
the `R` package `xml2`, and may be processed within `R` using any
methods compatible with such data, or may be processed by any other
software able to load the `XML` data directly from disk.

The first few lines of the XML data downloaded above look like this:

``` r

readLines ("buildings.osm") [1:6]
## [1] "<?xml version=\"1.0\" encoding=\"UTF-8\"?>"
## [2] "<osm version=\"0.6\" generator=\"Overpass API\">"
## [3] "  <note>The data included in this document is from www.openstreetmap.org. The data is made available under ODbL.</note>"
## [4] "  <meta osm_base=\"2017-03-07T09:28:03Z\"/>"
## [5] "  <node id=\"21593231\" lat=\"51.5149566\" lon=\"-0.1134203\"/>"
## [6] "  <node id=\"25378129\" lat=\"51.5135870\" lon=\"-0.1115193\"/>"
```

These data can be used in any other programs able to read and process
XML data, such as the open source GIS [QGIS](https://qgis.org/) or the
OSM data editor [JOSM](https://wiki.openstreetmap.org/wiki/JOSM). The
remainder of this vignette assumes that not only do you want to get OSM
data using R, you also want to import and eventually process it, using
R. For that you’ll need to import the data into a native R class.

As demonstrated above, downloaded data can be directly processed by
passing either filenames or the `R` objects containing those data to the
`osmdata_sf/sp()` functions:

``` r

dat_sp <- osmdata_sp (q, "buildings.osm")
dat_sf <- osmdata_sf (q, "buildings.osm")
```

### 3.2. The `osmdata_sf()` function

[`osmdata_sf()`](https://docs.ropensci.org/osmdata/reference/osmdata_sf.md)
returns OSM data in [Simple Features
(SF)](https://www.ogc.org/standards/sfo/) format, defined by the [Open
Geospatial Consortium](https://www.ogc.org), and implemented in the `R`
package [`sf`](https://cran.r-project.org/package=sf). This package
provides a direct interface to the `C++` [Graphical Data Abstraction
Library (GDAL)](https://gdal.org) which also includes a so-called
[‘driver’ for OSM
data](https://gdal.org/en/stable/drivers/vector/osm.html). This means
that OSM data may also be read directly with `sf`, rather than using
`osmdata`. In this case, data must first be saved to disk, which can be
readily achieved using
[`osmdata_xml()`](https://docs.ropensci.org/osmdata/reference/osmdata_xml.md)
described above, or through downloading directly from the [overpass
interactive query builder](https://overpass-turbo.eu).

The following example is based on this query:

``` r

opq (bbox = "Trentham, Australia") |>
    add_osm_feature (key = "name") |>
    osmdata_xml (filename = "trentham.osm")
```

`sf` can then read such data independent of `osmdata` though:

``` r

sf::st_read ("trentham.osm", layer = "points")
## Reading layer `points' from data source `trentham.osm' using driver `OSM'
## Simple feature collection with 38 features and 10 fields
## geometry type:  POINT
## dimension:      XY
## bbox:           xmin: 144.2894 ymin: -37.4846 xmax: 144.3893 ymax: -37.36012
## epsg (SRID):    4326
## proj4string:    +proj=longlat +datum=WGS84 +no_defs
```

The `GDAL` drivers used by `sf` can only load single ‘layers’ of
features, for example, `points`, `lines`, or `polygons`. In contrast,
`osmdata` loads all features simultaneously:

``` r

osmdata_sf (q, "trentham.osm")
## Object of class 'osmdata' with:
##                  $bbox : -37.4300874,144.2863388,-37.3500874,144.3663388
##         $overpass_call : The call submitted to the overpass API
##             $timestamp : [ Thus 5 May 2017 14:42:19 ]
##            $osm_points : 'sf' Simple Features Collection with 7106 points
##             $osm_lines : 'sf' Simple Features Collection with 263 linestrings
##          $osm_polygons : 'sf' Simple Features Collection with 38 polygons
##        $osm_multilines : 'sf' Simple Features Collection with 1 multilinestrings
##     $osm_multipolygons : 'sf' Simple Features Collection with 6 multipolygons
```

Even for spatial objects of the same type (the same ‘layers’ in `sf`
terminology), `osmdata` returns considerably more objects–7,166 points
compared .with just 38. The raw sizes of data returned can be compared
with:

``` r

s1 <- object.size (osmdata_sf (q, "trentham.osm")$osm_points)
s2 <- object.size (sf::st_read ("trentham.osm", layer = "points", quiet = TRUE))
as.numeric (s1 / s2)
## [1] 511.4193
```

And the `osmdata points` contain over 500 times as much data. The
primary difference between `sf/GDAL` and `osmdata` is that the former
returns only those objects unique to each category of spatial object.
Thus OSM nodes (`points` in `sf/osmdata` representations) include, in
`sf/GDAL` representation, only those points which are not part of any
other objects (such as lines or polygons). In contrast, the `osm_points`
object returned by `osmdata` includes all points regardless of whether
or not these are represented in other spatial objects. Similarly, `line`
objects in `sf/GDAL` exclude any lines that are part of other objects
such as `multipolygon` or `multiline` objects.

This processing of data by `sf/GDAL` has two important implications:

1.  An implicit hierarchy of spatial objects is enforced through
    including elements of objects only at their ‘highest’ level of
    representation, where `multipolygon` and `multiline` objects are
    assumed to be at ‘higher’ levels than `polyon` or `line` objects,
    and these in turn are at ‘higher’ levels than `point` objects.
    `osmdata` makes no such hierarchical assumptions.

2.  All OSM are structured by giving each object a unique identifier so
    that the components of any given object (the nodes of a line, for
    example, or the lines of a multipolygon) can be described simply by
    giving these identifiers. This enables the components of any OSM
    object to be examined in detail. The `sf/GDAL` representation
    obviates this ability through removing these IDs and reducing
    everything to geometries alone (which is, after all, why it is
    called ‘*Simple* Features’). This means, for example, that the
    `key-value` pairs of the `line` or `polygon` components of
    `multipolygon` can never be extracted from an `sf/GDAL`
    representation. In contrast, `osmdata` retains all unique
    identifiers for all OSM objects, and so readily enables, for
    example, the properties of all `point` objects of a `line` to be
    extracted.

Another reason why `osmdata` returns more data than `GDAL/sf` is that
the latter extracts only a restricted list of OSM `keys`, whereas
`osmdata` returns all `key` fields present in the requested data:

``` r

names (sf::st_read ("trentham.osm", layer = "points", quiet = TRUE)) # the keys
## [1] "osm_id"     "name"       "barrier"    "highway"
## [5] "ref"        "address"    "is_in"      "place"
## [9] "man_made"   "other_tags" "geometry"
```

``` r

names (osmdata_sf (q, "trentham.osm")$osm_points)
## [1] "osm_id"           "name"             "X_description_"   "X_waypoint_"
## [5] "addr.city"        "addr.housenumber" "addr.postcode"    "addr.street"
## [9] "amenity"          "barrier"          "denomination"     "foot"
## [13] "ford"             "highway"          "leisure"          "note_1"
## [17] "phone"            "place"            "railway"          "railway.historic"
## [21] "ref"              "religion"         "shop"             "source"
## [25] "tourism"          "waterway"         "geometry"
```

`key` fields which are not specified in a given set of OSM data are not
returned by `osmdata`, while `GDAL/sf` returns the same `key` fields
regardless of whether any values are specified.

``` r

addr <- sf::st_read ("trentham.osm", layer = "points", quiet = TRUE)$address
all (is.na (addr))
## TRUE
```

and `key=address` contains no data yet is still returned by `GDAL/sf`.

Finally, note that `osmdata` will generally extract OSM data
considerably faster than equivalent `sf/GDAL` routines (as detailed
[here](https://github.com/ropensci/osmdata/wiki/Timing-benchmarks)).

### 3.3. The `osmdata_sp()` function

As with
[`osmdata_sf()`](https://docs.ropensci.org/osmdata/reference/osmdata_sf.md)
described above, OSM data may be converted to `sp` format without using
`osmdata` via the `sf` functions demonstrated below:

``` r

dat <- sf::st_read ("buildings.osm", layer = "multipolygons", quiet = TRUE)
dat_sp <- as (dat, "Spatial")
class (dat_sp)
## [1] "SpatialPolygonsDataFrame"\nattr(,"package")\n[1] "sp"
```

These data are extracted using the GDAL, and so suffer all of the same
shortcomings mentioned above. Note differences in the amount of data
returned:

``` r

dim (dat_sp)
## [1] 560  25
```

``` r

dim (osmdata_sp (q, doc = "buildings.osm")$osm_polygons)
## [1] 566 114
```

``` r

dim (osmdata_sp (q, doc = "buildings.osm")$osm_multipolygons)
## [1] 15 52
```

## 4. Recursive searching

As described above, `osmdata` returns all data of each type and so
allows the components of any given spatial object to be examined in
their own right. This ability to extract, for example, all points of a
line, or all polygons which include a given set of points, is referred
to as *recursive searching*.

Recursive searching is not possible with `GDAL/sf`, because OSM
identifiers are removed, and only the unique data of each type of object
are retained. To understand both recursive searching and why it is
useful, note that OSM data are structured in three hierarchical levels:

1.  `nodes` representing spatial points

2.  `ways` representing lines, both as `polygons` (with connected ends)
    and non-polygonal `lines`

3.  `relations` representing more complex objects generally comprising
    collections of `ways` and/or `nodes`. Examples include
    `multipolygon relations` comprising an outer polygon (which may
    itself be made of several distinct `ways` which ultimately connect
    to form a single circle), and several inner polygons.

Recursive searching allows for objects within any one of these
hierarchical levels to be extracted based on components in any other
level. Recursive searching is performed in `osmdata` with the following
functions:

1.  [`osm_points()`](https://docs.ropensci.org/osmdata/reference/osm_points.md),
    which extracts all `point` or `node` objects

2.  [`osm_lines()`](https://docs.ropensci.org/osmdata/reference/osm_lines.md),
    which extracts all `way` objects that are `lines` (that are, that
    are not `polygons`)

3.  [`osm_polygons()`](https://docs.ropensci.org/osmdata/reference/osm_polygons.md),
    which extracts all `polygon` objects

4.  [`osm_multilines()`](https://docs.ropensci.org/osmdata/reference/osm_multilines.md),
    which extracts all `multiline` objects; and

5.  [`osm_multipolygons()`](https://docs.ropensci.org/osmdata/reference/osm_multipolygons.md),
    which extracts all `multipolygon` objects.

Each of these functions accepts as an argument a vector of OSM
identifiers. To demonstrate these functions, we first re-create the
example above of named objects from Trentham, Australia:

``` r

tr <- opq (bbox = "Trentham, Australia") |>
    add_osm_feature (key = "name") |>
    osmdata_sf ()
```

### 4.1. Example

Then imagine we are interested in the `osm_line` object describing the
‘Coliban River’:

``` r

i <- which (tr$osm_lines$name == "Coliban River")
coliban <- tr$osm_lines [i, ]
coliban [which (!is.na (coliban))]
## Simple feature collection with 1 feature and 3 fields
## geometry type:  LINESTRING
## dimension:      XY
## bbox:           xmin: 144.3235 ymin: -37.37162 xmax: 144.3335 ymax: 37.36366
## epsg (SRID):    4326
## proj4string:    +proj=longlat +datum=WGS84 +no_defs
##            osm_id          name waterway                       geometry
## 87104907 87104907 Coliban River    river LINESTRING(144.323471069336...
```

The locations of the points of this line can be extracted directly from
the `sf` object with:

``` r

coliban$geometry [[1]]
## LINESTRING(144.323471069336 -37.3716201782227, 144.323944091797 -37.3714790344238, 144.324356079102 -37.3709754943848, 144.324493408203 -37.3704833984375, 144.324600219727 -37.370174407959, 144.324981689453 -37.3697204589844, 144.325149536133 -37.369441986084, 144.325393676758 -37.3690567016602, 144.325714111328 -37.3686943054199, 144.326080322266 -37.3682441711426)
```

The output contains nothing other than geometries (because, to
reiterate, these are ‘*Simple* Features’), and no further information
regarding those points can be extracted. The Coliban River has a
waterfall in Trentham, and one of the `osm_points` objects describes
this waterfall. The information necessary to locate this waterfall is
removed from the `GDAL/sf` representation, but can be extracted with
`osmdata` with the following lines, noting that the OSM ID of the line
`coliban` is given by `rownames(coliban)`.

``` r

pts <- osm_points (tr, rownames (coliban))
wf <- pts [which (pts$waterway == "waterfall"), ]
wf [which (!is.na (wf))]
## Simple feature collection with 1 feature and 4 fields
## geometry type:  POINT
## dimension:      XY
## bbox:           xmin: 144.3246 ymin: -37.37017 xmax: 144.3246 ymax: -37.37017
## epsg (SRID):    4326
## proj4string:    +proj=longlat +datum=WGS84 +no_defs
##                osm_id           name    tourism  waterway
## 1013064837 1013064837 Trentham Falls attraction waterfall
##                                  geometry
## 1013064837 POINT(144.324600219727 -37....
```

This point could be used as the basis for further recursive searches.
For example, all `multipolygon` objects which include Trentham Falls
could be extracted with:

``` r

mp <- osm_multipolygons (tr, rownames (wf))
```

Although this returns no data in this case, it nevertheless demonstrates
the usefulness and ease of recursive searching with `osmdata`.

### 4.2 Relation example

A special type of OSM object is a relation. These can be defined by
their name, which can join many divers features into a single object.
The following example extracts the London Route Network Route 9, which
is composed of many (over 100) separate lines:

``` r

lcnr9 <- opq ("greater london uk") |>
    add_osm_feature (
        key = "name", value = "LCN 9",
        value_exact = FALSE
    ) |>
    osmdata_sp ()
sp::plot (lcnr9$osm_lines)
```

![](https://cloud.githubusercontent.com/assets/1825120/23709879/c98e2c2c-0412-11e7-86b8-1ffc95aab5a1.png)

## 5. Additional Functionality

This section briefly describes a few of additional functions, with
additional detail provided in the help files for each of these function.

1.  The
    [`trim_osmdata()`](https://docs.ropensci.org/osmdata/reference/trim_osmdata.md)
    function, as described above in the sub-section on bounding boxes,
    trims an `osmdata` object to within a defined bounding *polygon*,
    rather than bounding box.
2.  The
    [`opq_osm_id()`](https://docs.ropensci.org/osmdata/reference/opq_osm_id.md)
    function allows queries for particular OSM objects by their
    OSM-allocated ID values.
3.  The
    [`osm_poly2line()`](https://docs.ropensci.org/osmdata/reference/osm_poly2line.md)
    function converts all `$osm_polygons` items of an `osmdata` object
    to `$osm_lines`. These objects remain polygonal in form, through
    sharing identical start and end points, but can then be treated as
    simple lines. This is important for polygonal highways, which are
    automatically classified as `$osm_polygons` simply because they form
    closed loops. The function enables all highways to be grouped
    together (as `$osm_lines`) regardless of the form.
4.  The
    [`unique_osmdata()`](https://docs.ropensci.org/osmdata/reference/unique_osmdata.md)
    function removes redundant items from the different components of an
    `osmdata` object. A multilinestring, for example, is composed of
    multiple lines, and each line is composed of multiple points. For a
    multilinestring, an `osmdata` object will thus contain several
    `$osm_lines`, and for each of these several `$osm_points`. This
    function removes all of these redundant objects, so that
    `$osm_lines` only contains lines which are not part of any
    higher-level objects, and `$osm_points` only contains points which
    are not part of any higher-level objects.

A further additional function is the ability to extract data as
represented in the OSM database prior to a specified date, or within a
specified range of dates. This is achieved by passing one or both values
to the [`opq()`
function](https://docs.ropensci.org/osmdata/reference/opq.html) of
`datetime` and `datetime2`. The resultant data extracted with one or
more
[`add_osm_feature()`](https://docs.ropensci.org/osmdata/reference/add_osm_feature.md)
calls and an extraction function (`osmdata_sf/sp/sc/xml`) will then
contain only those data present prior to the specified date (when
`datetime` only given), or between the two specified dates (when both
`datetime` and `datetime2` given).

## 6. Related Packages

Eugster and Schlesinger (2012) describe `osmar`, an R package for
handling OSM data that enables visualisation, search and even
rudimentary routing operations. `osmar` is not user friendly or able to
download OSM data flexibly, as reported in an [early
tutorial](https://eprints.whiterose.ac.uk/77643/) comparing R and QGIS
for handling OSM data (Lovelace 2014). Note also that the `osmar`
package does not work at present, and can not be used for accessing OSM
data.

`osmdata` builds on two previous R packages: `osmplotr`, a package
[available from CRAN](https://cran.r-project.org/package=osmplotr) for
accessing and plotting OSM data (Padgham 2016) and `overpass`, a [GitHub
package](https://github.com/hrbrmstr/overpass) by Bob Rudis that
provides an R interface to the [overpass](https://overpass-api.de/) API.

## 7. References

Eugster, Manuel J a, and Thomas Schlesinger. 2012. “Osmar: OpenStreetMap
and R.” *The R Journal* 5 (1): 53–64.

Johnson, Peter A. 2017. “Models of Direct Editing of Government Spatial
Data: Challenges and Constraints to the Acceptance of Contributed Data.”
*Cartography and Geographic Information Science* 44 (2): 128–38.
<https://doi.org/10.1080/15230406.2016.1176536>.

Lovelace, Robin. 2014. “Harnessing Open Street Map Data with R and
QGIS.” *EloGeo*.

OpenStreetMap contributors. 2017. *Planet dump retrieved from
https://planet.osm.org* .
[Https://www.openstreetmap.org](https://www.openstreetmap.org).

Padgham, Mark. 2016. *Osmplotr: Customisable Images of OpenStreetMap
Data*. <https://cran.r-project.org/package=osmplotr>.

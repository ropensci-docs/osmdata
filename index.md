# osmdata

`osmdata` is an R package for accessing the data underlying
OpenStreetMap (OSM), delivered via the [Overpass
API](https://wiki.openstreetmap.org/wiki/Overpass_API). (Other packages
such as
[`OpenStreetMap`](https://cran.r-project.org/package=OpenStreetMap) can
be used to download raster tiles based on OSM data.)
[Overpass](https://overpass-turbo.eu) is a read-only API that extracts
custom selected parts of OSM data. Data can be returned in a variety of
formats, including as [Simple Features
(`sf`)](https://cran.r-project.org/package=sf), [Spatial
(`sp`)](https://cran.r-project.org/package=sp), or [Silicate
(`sc`)](https://github.com/hypertidy/silicate) objects. The package is
designed to allow access to small-to-medium-sized OSM datasets (see
[`osmextract`](https://github.com/ropensci/osmextract) for an approach
for reading-in bulk OSM data extracts).

> \[!IMPORTANT\]  
> You are responsible for following the [Usage
> Policy](https://wiki.openstreetmap.org/wiki/Overpass_API#Public_Overpass_API_instances).
> of the server you are connected to. See
> [`get_overpass_url()`](https://docs.ropensci.org/osmdata/reference/get_overpass_url.html)
> to check the server.
>
> You can modify the user agent of the requests by setting the option
> `osmdata.user_agent`:
>
> ``` r
>
> options(osmdata.user_agent = "my new user agent")
> ```

## Source code and installation

To install latest CRAN version:

``` r

install.packages ("osmdata")
```

Alternatively, install the development version with any one of the
following options:

``` r

# install.packages("remotes")
remotes::install_github ("ropensci/osmdata")
remotes::install_git ("https://codeberg.org/ropensci/osmdata")
remotes::install_gitlab ("ropensci/osmdata")
remotes::install_git ("https://git.sr.ht/~mpadge/osmdata")
```

These reflect the four locations at which the source code of this
package is currently hosted:

- <https://github.com/ropensci/osmdata>
- <https://codeberg.org/ropensci/osmdata>
- <https://gitlab.com/ropensci/osmdata>
- <https://git.sr.ht/~mpadge/osmdata>

The first of these,
[github.com/ropensci/osmdata](https://github.com/ropensci/osmdata), is
currently the primary location for the source code, and any questions or
suggestions should preferably be directed there, although we will also
respond to comments or issues on any of the other platforms.

To load the package and check the version:

``` r

library (osmdata)
#> Data (c) OpenStreetMap contributors, ODbL 1.0. https://www.openstreetmap.org/copyright
#> connected to: https://overpass-api.de/api/interpreter
packageVersion ("osmdata")
#> [1] '0.4.0.9002'
```

## Usage

[Overpass API](https://wiki.openstreetmap.org/wiki/Overpass_API) queries
can be built from a base query constructed with `opq` followed by
`add_osm_feature`. The corresponding OSM objects are then downloaded and
converted to [Simple Feature
(`sf`)](https://cran.r-project.org/package=sf) objects with
[`osmdata_sf()`](https://docs.ropensci.org/osmdata/reference/osmdata_sf.md),
[Spatial (`sp`)](https://cran.r-project.org/package=sp) objects with
[`osmdata_sp()`](https://docs.ropensci.org/osmdata/reference/osmdata_sp.md)
(DEPRECATED) or [Silicate (`sc`)](https://github.com/hypertidy/silicate)
objects with
[`osmdata_sc()`](https://docs.ropensci.org/osmdata/reference/osmdata_sc.md).
For example,

``` r

x <- opq (bbox = c (-0.27, 51.47, -0.20, 51.50)) |> # Chiswick Eyot in London, U.K.
    add_osm_feature (key = "name", value = "Thames", value_exact = FALSE) |>
    osmdata_sf ()
x
```

``` R
#> Object of class 'osmdata' with:
#>                  $bbox : 51.47,-0.27,51.5,-0.2
#>         $overpass_call : The call submitted to the overpass API
#>                  $meta : metadata including timestamp and version numbers
#>            $osm_points : 'sf' Simple Features Collection with 24548 points
#>             $osm_lines : 'sf' Simple Features Collection with 2219 linestrings
#>          $osm_polygons : 'sf' Simple Features Collection with 33 polygons
#>        $osm_multilines : 'sf' Simple Features Collection with 6 multilinestrings
#>     $osm_multipolygons : 'sf' Simple Features Collection with 3 multipolygons
```

OSM data can also be downloaded in OSM XML format with
[`osmdata_xml()`](https://docs.ropensci.org/osmdata/reference/osmdata_xml.md)
and saved for use with other software.

``` r

osmdata_xml(q1, "data.osm")
```

### Bounding Boxes

All `osmdata` queries begin with a bounding box defining the area of the
query. The [`getbb()`
function](https://docs.ropensci.org/osmdata/reference/getbb.html) can be
used to extract bounding boxes for specified place names.

``` r

getbb ("astana kazakhstan")
#>        min      max
#> x 71.21797 71.78519
#> y 50.85761 51.35111
```

The next step is to convert that to an overpass query object with the
[`opq()`
function](https://docs.ropensci.org/osmdata/reference/opq.html):

``` r

q <- opq (getbb ("astana kazakhstan"))
q <- opq ("astana kazakhstan") # identical result
```

It is also possible to use bounding polygons rather than rectangular
boxes:

``` r

b <- getbb ("bangalore", format_out = "polygon")
class (b)
#> [1] "list"
str (b)
#> List of 1
#>  $ relation/7902476:List of 1
#>   ..$ outer: num [1:4981, 1:2] 77.5 77.5 77.5 77.5 77.5 ...
```

### Features

The next step is to define features of interest using the
[`add_osm_feature()`
function](https://docs.ropensci.org/osmdata/reference/add_osm_feature.html).
This function accepts `key` and `value` parameters specifying desired
features in the [OSM key-vale
schema](https://wiki.openstreetmap.org/wiki/Map_Features). Multiple
[`add_osm_feature()`](https://docs.ropensci.org/osmdata/reference/add_osm_feature.md)
calls may be combined as illustrated below, with the result being a
logical AND operation, thus returning all amenities that are labelled
both as restaurants and also as pubs:

``` r

q <- opq ("portsmouth usa") |>
    add_osm_feature (key = "amenity", value = "restaurant") |>
    add_osm_feature (key = "amenity", value = "pub") # There are none of these
```

Features can also be requested by key only, in which case features with
any values for the specified key will be returned:

``` r

q <- opq ("portsmouth usa") |>
    add_osm_feature (key = "amenity")
```

Such key-only queries can, however, translate into requesting very large
data sets, and should generally be avoided in favour of more precise
key-value specifications.

Negation can also be specified by pre-pending an exclamation mark so
that the following requests all amenities that are NOT labelled as
restaurants and that are not labelled as pubs:

``` r

q <- opq ("portsmouth usa") |>
    add_osm_feature (key = "amenity", value = "!restaurant") |>
    add_osm_feature (key = "amenity", value = "!pub") # There are a lot of these
```

Additional arguments allow for more refined matching, such as the
following request for all pubs with “irish” in the name:

``` r

q <- opq ("washington dc") |>
    add_osm_feature (key = "amenity", value = "pub") |>
    add_osm_feature (
        key = "name", value = "irish",
        value_exact = FALSE, match_case = FALSE
    )
```

Logical OR combinations can be constructed using the separate
[`add_osm_features()`
function](https://docs.ropensci.org/osmdata/reference/add_osm_features.html).
The first of the above examples requests all features that are both
restaurants AND pubs. The following query will request data on
restaurants OR pubs:

``` r

q <- opq ("portsmouth usa") |>
    add_osm_features (features = c (
        "\"amenity\"=\"restaurant\"",
        "\"amenity\"=\"pub\""
    ))
```

The vector of `features` contains key-value pairs separated by an
[overpass “filter”
symbol](https://wiki.openstreetmap.org/wiki/Overpass_API/Overpass_QL#By_tag_.28has-kv.29)
such as `=`, `!=`, or `~`. Each key and value must be enclosed in
escape-delimited quotations as shown above.

Full lists of available features and corresponding tags are available in
the functions
[`?available_features`](https://docs.ropensci.org/osmdata/reference/available_features.html)
and
[`?available_tags`](https://docs.ropensci.org/osmdata/reference/available_tags.html).

### Data Formats

An overpass query constructed with the
[`opq()`](https://docs.ropensci.org/osmdata/reference/opq.md) and
[`add_osm_feature()`](https://docs.ropensci.org/osmdata/reference/add_osm_feature.md)
functions is then sent to the [overpass
server](https://overpass-turbo.eu) to request data. These data may be
returned in a variety of formats, currently including:

1.  XML data (downloaded locally) via
    [`osmdata_xml()`](https://docs.ropensci.org/osmdata/reference/osmdata_xml.html);
2.  [Simple Features (sf)](https://cran.r-project.org/package=sf) format
    via
    [`osmdata_sf()`](https://docs.ropensci.org/osmdata/reference/osmdata_sf.html);
3.  [R Spatial (sp)](https://cran.r-project.org/package=sp) format via
    [`osmdata_sp()`](https://docs.ropensci.org/osmdata/reference/osmdata_sp.html)
    (DEPRECATED);
4.  [Silicate (SC)](https://github.com/hypertidy/silicate) format via
    [`osmdata_sc()`](https://docs.ropensci.org/osmdata/reference/osmdata_sc.html);
    and
5.  `data.frame` format via
    [`osmdata_data_frame()`](https://docs.ropensci.org/osmdata/reference/osmdata_data_frame.html).

### Additional Functionality

Data may also be trimmed to within a defined polygonal shape with the
[`trim_osmdata()`](https://docs.ropensci.org/osmdata/reference/trim_osmdata.html)
function. Full package functionality is described on the
[website](https://docs.ropensci.org/osmdata/)

## Citation

``` r

citation ("osmdata")
#> To cite osmdata in publications use:
#> 
#>   Mark Padgham, Bob Rudis, Robin Lovelace, Maëlle Salmon (2017).
#>   "osmdata." _Journal of Open Source Software_, *2*(14), 305.
#>   doi:10.21105/joss.00305 <https://doi.org/10.21105/joss.00305>.
#>   <https://joss.theoj.org/papers/10.21105/joss.00305>.
#> 
#> A BibTeX entry for LaTeX users is
#> 
#>   @Article{,
#>     title = {osmdata},
#>     author = {{Mark Padgham} and {Bob Rudis} and {Robin Lovelace} and {Maëlle Salmon}},
#>     journal = {Journal of Open Source Software},
#>     year = {2017},
#>     volume = {2},
#>     number = {14},
#>     pages = {305},
#>     month = {jun},
#>     publisher = {The Open Journal},
#>     url = {https://joss.theoj.org/papers/10.21105/joss.00305},
#>     doi = {10.21105/joss.00305},
#>   }
```

## Data licensing

All data that you access using `osmdata` is licensed under
[OpenStreetMap’s license, the Open Database
Licence](https://osmfoundation.org/wiki/Licence). Any derived data and
products must also carry the same licence. You should make sure you
understand that licence before publishing any derived datasets.

## Other approaches

- [osmextract](https://docs.ropensci.org/osmextract/) is an R package
  for downloading and importing compressed ‘extracts’ of OSM data
  covering large areas (e.g. all roads in a country). The package
  represents data in [`sf`](https://github.com/r-spatial/sf) format
  only, and only allows a single “layer” (such as points, lines, or
  polygons) to be read at one time. It is nevertheless recommended over
  osmdata for large queries of single layers, or where relationships
  between layers are not important.

- [osmapiR](https://docs.ropensci.org/osmapiR/) is an R interface to the
  [OpenStreetMap API v0.6](https://wiki.openstreetmap.org/wiki/API_v0.6)
  for fetching and saving raw geodata from/to the OpenStreetMap
  database. This package allows access to OSM maps data as well as map
  notes, GPS traces, changelogs, and users data. `osmapiR` enables
  editing or exploring the history of OSM objects, and is not intended
  to access OSM map data for other purposes (unlike the osmdata or
  osmextract packages).

## Code of Conduct

Please note that this package is released with a [Contributor Code of
Conduct](https://ropensci.org/code-of-conduct/). By contributing to this
project, you agree to abide by its terms.

## Contributors

All contributions to this project are gratefully acknowledged using the
[`allcontributors` package](https://github.com/ropensci/allcontributors)
following the [allcontributors](https://allcontributors.org)
specification. Contributions of any kind are welcome!

### Code

[TABLE]

### Issue Authors

[TABLE]

### Issue Contributors

[TABLE]

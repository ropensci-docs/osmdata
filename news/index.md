# Changelog

## osmdata (development version)

## osmdata 0.4.1

### Major changes

- Add
  [`list_overpass_urls()`](https://docs.ropensci.org/osmdata/reference/list_overpass_urls.md)
  and prefer a responsive Overpass API mirror, checked via a new
  `check_status()` helper, on package load and in `overpass_query()`
  ([\#428](https://github.com/ropensci/osmdata/issues/428)).

### Minor changes

- Fix
  [`osmdata_sf()`](https://docs.ropensci.org/osmdata/reference/osmdata_sf.md)
  for points without metadata
  ([\#426](https://github.com/ropensci/osmdata/issues/426)).
- Add user agent in requests to servers
  ([\#430](https://github.com/ropensci/osmdata/issues/430)).
- Update dead links
  ([\#433](https://github.com/ropensci/osmdata/issues/433)).

## osmdata 0.4.0

CRAN release: 2026-06-15

### Breaking changes

- [`getbb()`](https://docs.ropensci.org/osmdata/reference/getbb.md)
  throws a warning instead of an error for empty results and return the
  (empty) result with the expected type according to `format_out`
  parameter ([\#394](https://github.com/ropensci/osmdata/issues/394)).
- Timestamp in metadata has a POSIXct time. Before it was character with
  a non-standard locale dependent date format
  ([\#416](https://github.com/ropensci/osmdata/issues/416)).

### Major changes

- [`getbb()`](https://docs.ropensci.org/osmdata/reference/getbb.md) can
  query wikidata ids for OSM relations properties (P402) (idea from
  [@mhpob](https://github.com/mhpob) in
  [\#401](https://github.com/ropensci/osmdata/issues/401), implemented
  in [\#403](https://github.com/ropensci/osmdata/issues/403)).
- Add
  [`filter_osm_user()`](https://docs.ropensci.org/osmdata/reference/filter_osm_user.md)
  to add user filter to `overpass_queries` objects
  ([\#414](https://github.com/ropensci/osmdata/issues/414)).

### Minor changes

- Fixed
  [`osm_elevation()`](https://docs.ropensci.org/osmdata/reference/osm_elevation.md)
  function after raster -\> terra upgrade
  ([\#389](https://github.com/ropensci/osmdata/issues/389); thanks to
  [@Aloniss](https://github.com/Aloniss)).
- Implement described viewbox parameter in
  [`getbb()`](https://docs.ropensci.org/osmdata/reference/getbb.md)
  ([\#402](https://github.com/ropensci/osmdata/issues/402)).
- Implement `osmdata_sf(..., out = "meta")`& metadata columns (also for
  `osmdata_data.frame()`) are encoded in utf8 (`osm_user`) and formatted
  as POSIXct (`osm_timestamp`)
  ([\#405](https://github.com/ropensci/osmdata/issues/405)).
- Remove `codemeta.json`
  ([\#410](https://github.com/ropensci/osmdata/issues/410)).

## osmdata 0.3.0

CRAN release: 2025-08-23

### Breaking changes

- Remove `magrittr` from imports. User code relaying on reexported pipe
  `%>%` from `osmdata` must explicitly load it with
  [`library(magrittr)`](https://magrittr.tidyverse.org). Code examples,
  tests and vignettes now use the pipe from base (`|>`) available since
  R 4.1 ([\#361](https://github.com/ropensci/osmdata/issues/361))
- `getbb(..., format_out = "polygon")` return polygons following
  \[<https://www.ogc.org/standards/sfa/>\]. Polygons are defined by a
  list of matrices of coordinates. The first ring defines the exterior
  boundary, and the following rings define holes if present. Also fix
  `getbb(..., format_out = "sf_polygon")` returning each (multi)polygon
  as a row in an `sf` object. Before, every ring was an independent
  polygon, even for holes or multipolygons, and for
  `format_out = "sf_polygon"`, the features were split in a list with
  polygons in one item and multipolygons in another
  ([\#378](https://github.com/ropensci/osmdata/issues/378)).

### Major changes

- Implemented `c.osmdata_sc` method to join `osmdata_sc` objects
  ([\#333](https://github.com/ropensci/osmdata/issues/333))
- Depends on R \>= 4.1 to use the base pipe (`|>`) in examples and
  vignettes ([\#371](https://github.com/ropensci/osmdata/issues/371))
- Deprecate `nodes_only` argument in
  [`opq()`](https://docs.ropensci.org/osmdata/reference/opq.md).
  Superseded by argument `osm_types`
  ([\#370](https://github.com/ropensci/osmdata/issues/370))
- Deprecate `osmdata_sp`
  ([\#372](https://github.com/ropensci/osmdata/issues/372))
- Pre-prend class names `osmdata_sf` and `osmdata` rather than append;
  thanks to [@agila5](https://github.com/agila5)
  ([\#373](https://github.com/ropensci/osmdata/issues/373))
- Add `osmadata_data.frame` class to
  [`osmdata_data_frame()`](https://docs.ropensci.org/osmdata/reference/osmdata_data_frame.md)
  results ([\#378](https://github.com/ropensci/osmdata/issues/378))
- Reimplement
  [`trim_osmdata()`](https://docs.ropensci.org/osmdata/reference/trim_osmdata.md)
  using `sf` instead of `sp`. Now, it checks the full geometry instead
  of just the points to determine if an object is properly contained in
  the bbox (only for `osmdata_sf` objects, `osdmata_sc` still wrong)
  ([\#382](https://github.com/ropensci/osmdata/issues/382)).

### Minor changes

- Improved `get_bb(..., format_out = "sf_polygon")` to return full
  metadata along with geometries
  ([\#338](https://github.com/ropensci/osmdata/issues/338) thanks to
  [@RegularnaMatrica](https://github.com/RegularnaMatrica))
- Mention key-only feature requests in README
  ([\#342](https://github.com/ropensci/osmdata/issues/342) thanks to
  [@joostschouppe](https://github.com/joostschouppe))
- Merge any columns in
  [`osmdata_sf()`](https://docs.ropensci.org/osmdata/reference/osmdata_sf.md)
  with mixed-case duplicated names
  ([\#348](https://github.com/ropensci/osmdata/issues/348))
- Set encoding to UTF-8 for tags and user names
  ([\#347](https://github.com/ropensci/osmdata/issues/347))
- Document the use of the input query as character strings for
  `osmdata_*()`
  ([\#349](https://github.com/ropensci/osmdata/issues/349))
- Consistent `NA` values throughout all multi-\* matrices returned by
  [`osmdata_sf()`](https://docs.ropensci.org/osmdata/reference/osmdata_sf.md)
  ([\#355](https://github.com/ropensci/osmdata/issues/355))
- Fix dates and remove `lubridate` from imports
  ([\#360](https://github.com/ropensci/osmdata/issues/360))
- Restructure class definitions of
  [`osmdata_sf()`](https://docs.ropensci.org/osmdata/reference/osmdata_sf.md)
  and
  [`osmdata_sc()`](https://docs.ropensci.org/osmdata/reference/osmdata_sc.md)
  objects ([\#373](https://github.com/ropensci/osmdata/issues/373),
  [\#374](https://github.com/ropensci/osmdata/issues/374))
- Revert added `osmdata` class to
  [`osmdata_data_frame()`](https://docs.ropensci.org/osmdata/reference/osmdata_data_frame.md)
  and
  [`osmdata_sc()`](https://docs.ropensci.org/osmdata/reference/osmdata_sc.md) +
  Fix docs to better represent classes accepted by
  [`trim_osmdata()`](https://docs.ropensci.org/osmdata/reference/trim_osmdata.md),
  [`osm_poly2line()`](https://docs.ropensci.org/osmdata/reference/osm_poly2line.md)
  and extract function
  ([\#380](https://github.com/ropensci/osmdata/issues/380))
- Use `terra` functions instead of `raster` (obsolete) in
  [`osm_elevation()`](https://docs.ropensci.org/osmdata/reference/osm_elevation.md)
  ([\#383](https://github.com/ropensci/osmdata/issues/383))
- Joan Maspons is the new maintainer
  ([\#384](https://github.com/ropensci/osmdata/issues/384)).

## osmdata 0.2.5

CRAN release: 2023-08-14

### Major changes

- v0.2.4 was removed without notice from CRAN because of
  [\#329](https://github.com/ropensci/osmdata/issues/329); this is a
  rapid re-submission

## osmdata 0.2.4

CRAN release: 2023-08-11

### Minor changes

- Bug fix to stop getbb call to Nominatim returning 405 error
  ([\#328](https://github.com/ropensci/osmdata/issues/328))

## osmdata 0.2.3

CRAN release: 2023-06-01

### Minor changes

- Fix failing test due to changes to ‘sp’ moving towards deprecation.

## osmdata 0.2.2

CRAN release: 2023-04-24

### Major changes:

- `osmdata_data_frame` adds columns `osm_center_lat` and
  `osm_center_lon` for `out * center;` queries
  ([\#316](https://github.com/ropensci/osmdata/issues/316),
  [\#319](https://github.com/ropensci/osmdata/issues/319)).
- Add parameters from `opq` to `opq_osm_id`: out, datetime, datetime2,
  adiff, timeout and memsize
  ([\#320](https://github.com/ropensci/osmdata/issues/320))
- Fix
  [`available_tags()`](https://docs.ropensci.org/osmdata/reference/available_tags.md)
  function which no longer worked
  ([\#322](https://github.com/ropensci/osmdata/issues/322) thanks to
  [@boiled-data](https://github.com/boiled-data))
- Implement `out:csv` queries
  ([\#321](https://github.com/ropensci/osmdata/issues/321)).

### Minor changes

- Fix queries with `!match_case` and only one value
  ([\#317](https://github.com/ropensci/osmdata/issues/317)).
- Fix queries with multiple features & multiple osm_types
  ([\#318](https://github.com/ropensci/osmdata/issues/318)).

## osmdata 0.2.1

CRAN release: 2023-02-24

### Major changes:

- Very soft deprecation of `nodes_only` parameter in `opq`
  ([\#308](https://github.com/ropensci/osmdata/issues/308),
  [\#312](https://github.com/ropensci/osmdata/issues/312)).

### Minor changes

- Couple of minor memory leak bug fixes in `osmdata_data_frame` C++
  code.

## osmdata 0.2.0

CRAN release: 2023-02-09

This release welcomes a new package author
[@jmaspons](https://github.com/jmaspons). The lists of changes here
gives an overview of the amazing work he has contributed to this new
major version.

### Major changes:

- New
  [`osmdata_data_frame()`](https://docs.ropensci.org/osmdata/reference/osmdata_data_frame.md)
  function to return non-spatial `data.frame` structures directly from
  overpass; thanks to [@jmaspons](https://github.com/jmaspons)
  ([\#285](https://github.com/ropensci/osmdata/issues/285)).
- Improved `add_osm_features` so that key-values pairs can be submitted
  as a list, rather than escape-delimited character strings; thanks to
  [@elipousson](https://github.com/elipousson)
  ([\#277](https://github.com/ropensci/osmdata/issues/277),
  [\#278](https://github.com/ropensci/osmdata/issues/278)).
- [`opq()`](https://docs.ropensci.org/osmdata/reference/opq.md) can now
  utilise overpass ability to filter results by area; thanks to
  [@jmaspons](https://github.com/jmaspons)
  ([\#286](https://github.com/ropensci/osmdata/issues/286)).
- [`opq()`](https://docs.ropensci.org/osmdata/reference/opq.md) now has
  additional “out” parameter to control the kinds of data returned by
  overpass; thanks to [@jmaspons](https://github.com/jmaspons)
  ([\#288](https://github.com/ropensci/osmdata/issues/288)).
- [`opq()`](https://docs.ropensci.org/osmdata/reference/opq.md) now has
  additional “osm_types” parameter to provide finer control of which
  kinds of data are returned by overpass; thanks to
  [@jmaspons](https://github.com/jmaspons)
  ([\#295](https://github.com/ropensci/osmdata/issues/295)).
- Fix key modifications for non-valid column names and handle duplicated
  column names in `osmdata_*` functions; by
  [@jmaspons](https://github.com/jmaspons)
  ([\#303](https://github.com/ropensci/osmdata/issues/303))
- @elipousson is new package contributor, thanks to the above work.
- @jmaspons is new package author, thanks to
  [\#285](https://github.com/ropensci/osmdata/issues/285) (plus most of
  the above, and a whole lot more!)

### Minor changes:

- Downgraded `sp` from “Imports” to “Suggests”; thanks to
  [@jmaspons](https://github.com/jmaspons)
  ([\#302](https://github.com/ropensci/osmdata/issues/302))
- Improved `osm_osm_id()` to accept vectors of ids and types; thanks to
  [@jmaspons](https://github.com/jmaspons)
  ([\#268](https://github.com/ropensci/osmdata/issues/268),
  [\#282](https://github.com/ropensci/osmdata/issues/282),
  [\#283](https://github.com/ropensci/osmdata/issues/283))
- “get-osmdata.R” file now split into several smaller and more
  manageable files
  ([\#306](https://github.com/ropensci/osmdata/issues/306), thanks to
  [@jmaspons](https://github.com/jmaspons))

## osmdata 0.1.10

CRAN release: 2022-06-09

### Major changes:

- Changed httr dependency for httr2
  ([\#272](https://github.com/ropensci/osmdata/issues/272))
- Removed two authors of code formerly including for stubbing results;
  which is now done via `httptest2` package.

### Minor changes:

- Moved jsonlite from Imports to Suggests (now only used in tests).

## osmdata 0.1.9

CRAN release: 2022-01-26

### Major changes:

- New function `opq_around` to query features within a specified radius
  *around* a defined location; thanks to
  [@barryrowlingson](https://github.com/barryrowlingson) via
  [\#199](https://github.com/ropensci/osmdata/issues/199) and
  [@maellecoursonnais](https://github.com/maellecoursonnais) via
  [\#238](https://github.com/ropensci/osmdata/issues/238)
- New vignette on splitting large queries thanks to
  [@Machin6](https://github.com/Machin6) (via
  [\#262](https://github.com/ropensci/osmdata/issues/262))

### Minor changes:

- New dependency on `reproj` package, so that
  [`trim_osmdata()`](https://docs.ropensci.org/osmdata/reference/trim_osmdata.md)
  can be applied to re-projected coordinates.

## osmdata 0.1.8

CRAN release: 2021-10-17

### Minor changes:

- Fix some failing CRAN checks (no change to functionality)

## osmdata 0.1.7

CRAN release: 2021-10-07

### Minor changes:

- `add_osm_feature` bug fix to revert AND behaviour
  ([\#240](https://github.com/ropensci/osmdata/issues/240) thanks to
  [@anthonynorth](https://github.com/anthonynorth))

## osmdata 0.1.6

CRAN release: 2021-07-28

### Major changes:

- New function `add_osm_features` to enable OR-combinations of features
  in single queries.

## osmdata 0.1.5

CRAN release: 2021-03-22

### Minor changes:

- Bug fix in
  [`getbb()`](https://docs.ropensci.org/osmdata/reference/getbb.md) via
  [\#232](https://github.com/ropensci/osmdata/issues/232), thanks to
  [@changwoo-lee](https://github.com/changwoo-lee)
- hard-code WKT string for EPSG:4326, to avoid obsolete proj4strings
  ([\#218](https://github.com/ropensci/osmdata/issues/218))
- bug fix in `print` method via
  [\#236](https://github.com/ropensci/osmdata/issues/236); thanks to
  [@odeleongt](https://github.com/odeleongt)

## osmdata 0.1.4

CRAN release: 2020-11-03

### Major changes:

- New `osm_enclosing()` function; thanks to
  [@barryrowlingson](https://github.com/barryrowlingson) via
  [\#199](https://github.com/ropensci/osmdata/issues/199)
- [`opq()`](https://docs.ropensci.org/osmdata/reference/opq.md) now has
  additional `datetime` and `datetime2` parameters which can be used to
  extract historical data prior to `datetime`, or differences between
  two datetimes by specifying `datetime2`; thanks to
  [@neogeomat](https://github.com/neogeomat) for the idea in issue#179.
- opq() also has additional `nodes_only` parameter to return nodes as
  points only, for efficient extraction of strictly point-based OSM
  data; thanks to [@gdkrmr](https://github.com/gdkrmr) for the idea in
  issue#221.

### Minor changes:

- New contributor Enrico Spinielli
  ([@espinielli](https://github.com/espinielli)), via
  [\#207](https://github.com/ropensci/osmdata/issues/207),
  [\#210](https://github.com/ropensci/osmdata/issues/210),
  [\#211](https://github.com/ropensci/osmdata/issues/211),
  [\#212](https://github.com/ropensci/osmdata/issues/212) - Thanks!

## osmdata 0.1.3

CRAN release: 2020-02-09

### Major changes:

- `osmdata_pbf` function removed as the overpass server no longer
  provides the experimental API for pbf-format data.
- Remove deprecated `add_feature()` function; entirely replaced by
  [`add_osm_feature()`](https://docs.ropensci.org/osmdata/reference/add_osm_feature.md).
- `get_bb()` with polygon output formats now returns ALL polygon and
  multipolygon objects by default (issue#195)

### Minor changes:

- New Contributors: Andrea Gilardi
  ([@agila5](https://github.com/agila5))
- Bug fix for issue#205

## osmdata 0.1.2

CRAN release: 2019-12-14

### Major changes:

- New function `unname_osmdata_sf`, to remove row names from `sf`-format
  geometry objects that may cause issues with some plotting routines
  such as leaflet.

### Minor changes:

- `getbb` now allows arbitrary `featuretype` specification, no longer
  just those pertaining to settlement forms.
- available_tags returns tags with underscore precisely as required for
  `add_osm_feature` - previous version returned text values with spaces
  instead of underscore.
- Fix bug in `osmdata_sf` for data with no names and/or no key-val pairs
- Fix bug in `trim_osmdata` for multi\* objects; thanks to
  [@stragu](https://github.com/stragu)
- Implement `trim_osmdata.sc` method
- retry httr calls to nominatim, which has lately been timing out quite
  often

## osmdata 0.1.1

CRAN release: 2019-05-22

### Minor changes:

- bug fix in `trim_osmdata` function

## osmdata 0.1.0

CRAN release: 2019-04-25

### Major changes:

- New function, `osm_elevation` to insert elevation data into
  `SC`-format data returned by `osmdata_sc` function.
- New vignette on `osmdata_sc` function and elevation data.
- [`opq()`](https://docs.ropensci.org/osmdata/reference/opq.md) function
  now accepts polygonal bounding boxes generated with
  `getbb(..., format_out = "polygon")`.

## osmdata 0.0.10

CRAN release: 2019-03-21

### Minor changes:

- Bug fix for vectorized lists of values in `add_osm_feature`, so only
  listed items are returns (see
  [\#139](https://github.com/ropensci/osmdata/issues/139); thanks
  [@loreabad6](https://github.com/loreabad6))
- But fix to ensure all `sf` `data.frame` objects have
  `stringsAsFactors = FALSE`

## osmdata 0.0.9

CRAN release: 2018-12-19

### Major changes:

- New function `osmdata_sc` to return data in `silicate::SC` format (see
  github.com/hypertidy/silicate; this also requires additional
  dependency on `tibble`)
- Structure of `osmdata` object modified to replace former `$timestamp`
  field with `$meta` field containing a list of `$timestamp`,
  `$OSM_version` (currently 0.6), and `$overpass_version`.
- add_osm_feature() now accepts vectors of multiple values (see
  [\#139](https://github.com/ropensci/osmdata/issues/139)).
- osmdata_sf() objects default to character vectors, not factors (see
  [\#44](https://github.com/ropensci/osmdata/issues/44)).

### Minor changes:

- vignette updated
- Overpass URL now randomly selected from the four primary servers (see
  <https://wiki.openstreetmap.org/wiki/Overpass_API#Public_Overpass_API_instances>),
  thanks to [@JimShady](https://github.com/JimShady).
- bug fix for osmdata_sp() (see
  [\#56](https://github.com/ropensci/osmdata/issues/56))
- osmdata_sp() fixed to return osm_id values (see
  [\#131](https://github.com/ropensci/osmdata/issues/131); thanks
  [@JimShady](https://github.com/JimShady)).

## osmdata 0.0.8

CRAN release: 2018-10-22

- Fix bug in `trim_osmdata` so that all sf attributes are reinstated,
  and also issue message that sf-preload is necessary for this function
- Fix bug with opq (key_exact = FALSE) so value_exact is always also set
  to FALSE

## osmdata 0.0.7

CRAN release: 2018-05-17

- Fix bug in `c` method so it works when `sf` not loaded
- Fix bug in overpass query syntax to match new QL requirements

## osmdata 0.0.6

CRAN release: 2018-02-22

- Add new function ‘osm_poly2line()’ to coerce the
  ‘osmdata$`odm_polygons' object
  for 'osmdata_sf' objects to lines, and append to 'osmdata`$osm_lnes’.
  This is important for street networks (‘add_osm_objects (key =
  “highway”)’), which are otherwise separated between these two
  components.
- Add new function `opq_osm_id` to query by OSM identifier alone
- Add `timeout` and `memsize` options to
  [`opq()`](https://docs.ropensci.org/osmdata/reference/opq.md) to
  improve handling large queries.
- Return useful information from overpass server when it returns neither
  error nor useful data
- Make C++ code interruptible so long processing can be cancelled
- Fix minor yet important C++ code lines that prevented package being
  used as dependency by other packages on some systems

## osmdata 0.0.5

CRAN release: 2017-08-13

- Add extraction of bounding polygons with
  `getbb (..., format_out = "polygon")`
- Add `trim_osmdata` function to trim an `osmdata` object to within a
  bounding polygon (thanks [@sytpp](https://github.com/sytpp))
- Add `unique_osmdata` function which reduces each component of an
  `osmdata` object only to unique elements (so `$osm_points`, for
  example, only contains points that are not represented in other -
  line, polygon, whatever - objects).
- Rename `add_feature` to `add_osm_feature` (and deprecate old version)

## osmdata 0.0.4

CRAN release: 2017-06-21

- Enable alternative overpass API services through
  [`get_overpass_url()`](https://docs.ropensci.org/osmdata/reference/get_overpass_url.md)
  and
  [`set_overpass_url()`](https://docs.ropensci.org/osmdata/reference/set_overpass_url.md)
  functions
- Extend and improve vignette

## osmdata 0.0.3

CRAN release: 2017-06-13

- Change tests only, no functional difference

## osmdata 0.0.2

CRAN release: 2017-06-12

- Rename function
  [`opq_to_string()`](https://docs.ropensci.org/osmdata/reference/opq_string.md)
  to
  [`opq_string()`](https://docs.ropensci.org/osmdata/reference/opq_string.md)

## osmdata 0.0.1 (19 May 2017)

CRAN release: 2017-05-19

- Remove configure and Makevars files
- Fix tests

## osmdata 0.0.0 (18 May 2017)

CRAN release: 2017-05-18

- Initial CRAN release

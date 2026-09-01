# osmdata class def

osmdata class def

## Usage

``` r
osmdata(
  bbox = NULL,
  overpass_call = NULL,
  meta = NULL,
  osm_points = NULL,
  osm_lines = NULL,
  osm_polygons = NULL,
  osm_multilines = NULL,
  osm_multipolygons = NULL
)
```

## Arguments

- bbox:

  bounding box

- overpass_call:

  overpass_call

- meta:

  metadata of overpass query, including timestamps and version numbers

- osm_points:

  OSM nodes as sf Simple Features Collection of points or sp
  SpatialPointsDataFrame

- osm_lines:

  OSM ways sf Simple Features Collection of linestrings or sp
  SpatialLinesDataFrame

- osm_polygons:

  OSM ways as sf Simple Features Collection of polygons or sp
  SpatialPolygonsDataFrame

- osm_multilines:

  OSM relations as sf Simple Features Collection of multilinestrings or
  sp SpatialLinesDataFrame

- osm_multipolygons:

  OSM relations as sf Simple Features Collection of multipolygons or sp
  SpatialPolygonsDataFrame

## Note

Class constructor should never be used directly, and is only exported to
provide access to the print method

## Examples

``` r
# This function should not need to be called directly!
osmdata ()
#>                  $bbox : 
#>         $overpass_call : The call submitted to the overpass API
#>                  $meta : metadata including timestamp and version numbers
#>            $osm_points : NULL
#>             $osm_lines : NULL
#>          $osm_polygons : NULL
#>        $osm_multilines : NULL
#>     $osm_multipolygons : NULL
```

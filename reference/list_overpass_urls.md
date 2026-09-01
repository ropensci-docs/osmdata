# list_overpass_urls

List public Overpass API URLs. These are mirrors sampled by default when
the package loads.

## Usage

``` r
list_overpass_urls()
```

## Value

A character vector of Overpass API interpreter URLs.

## Details

For further details, see
<https://wiki.openstreetmap.org/wiki/Overpass_API#Public_Overpass_API_instances>
and <https://github.com/ropensci/osmdata/pull/149>.

## See also

Other overpass:
[`get_overpass_url()`](https://docs.ropensci.org/osmdata/reference/get_overpass_url.md),
[`set_overpass_url()`](https://docs.ropensci.org/osmdata/reference/set_overpass_url.md)

## Examples

``` r
list_overpass_urls ()
#> [1] "https://overpass-api.de/api/interpreter"                
#> [2] "https://maps.mail.ru/osm/tools/overpass/api/interpreter"
```

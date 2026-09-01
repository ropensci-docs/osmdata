# get_overpass_url

Return the URL of the specified overpass API. Default is
`https://overpass-api.de/api/interpreter/`.

## Usage

``` r
get_overpass_url()
```

## Value

The overpass API URL

## See also

Other overpass:
[`list_overpass_urls()`](https://docs.ropensci.org/osmdata/reference/list_overpass_urls.md),
[`set_overpass_url()`](https://docs.ropensci.org/osmdata/reference/set_overpass_url.md)

## Examples

``` r
get_overpass_url ()
#> [1] "https://maps.mail.ru/osm/tools/overpass/api/interpreter"
```

# set_overpass_url

Set the URL of the specified overpass API. Possible APIs with global
coverage are returned by
[`list_overpass_urls()`](https://docs.ropensci.org/osmdata/reference/list_overpass_urls.md).

## Usage

``` r
set_overpass_url(overpass_url)
```

## Arguments

- overpass_url:

  The desired overpass API URL

## Value

The overpass API URL

## Details

For further details, see
<https://wiki.openstreetmap.org/wiki/Overpass_API>

## See also

Other overpass:
[`get_overpass_url()`](https://docs.ropensci.org/osmdata/reference/get_overpass_url.md),
[`list_overpass_urls()`](https://docs.ropensci.org/osmdata/reference/list_overpass_urls.md)

## Examples

``` r
if (FALSE) { # \dontrun{
set_overpass_url (list_overpass_urls () [1])
} # }
```

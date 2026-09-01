# List tags associated with a feature

List tags associated with a feature

## Usage

``` r
available_tags(feature)
```

## Arguments

- feature:

  feature to retrieve

## Value

character vector of all known tags for a feature

## Note

requires internet access

## References

<https://wiki.openstreetmap.org/wiki/Map_Features>

## See also

Other osminfo:
[`available_features()`](https://docs.ropensci.org/osmdata/reference/available_features.md)

## Examples

``` r
if (FALSE) { # \dontrun{
available_tags ("aerialway")
} # }
```

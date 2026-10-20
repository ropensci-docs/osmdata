# Deprecated functions and arguments in osmdata

These functions and arguments have been deprecated and will be removed
in a future release.

## Deprecated arguments

- `nodes_only = TRUE` (in
  [`opq()`](https://docs.ropensci.org/osmdata/reference/opq.md)):

  This argument has been replaced by `osm_types = "node"`. Since version
  0.3, using `nodes_only` will produce a deprecation warning.

## Deprecated functions

- [`osmdata_sp()`](https://docs.ropensci.org/osmdata/reference/osmdata_sp.md):

  Please use
  [`osmdata_sf()`](https://docs.ropensci.org/osmdata/reference/osmdata_sf.md)
  or
  [`osmdata_sc()`](https://docs.ropensci.org/osmdata/reference/osmdata_sc.md)
  instead. Since version 0.3, using
  [`osmdata_sp()`](https://docs.ropensci.org/osmdata/reference/osmdata_sp.md)
  will produce a deprecation warning.

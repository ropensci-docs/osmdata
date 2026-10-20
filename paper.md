# Summary

`osmdata` imports OpenStreetMap (OSM) data into R as either Simple
Features or `R` Spatial objects, respectively able to be processed with
the R packages `sf` and `sp`. OSM data are extracted from the Overpass
API and processed with very fast C++ routines for return to R. The
package enables simple Overpass queries to be constructed without the
user necessarily understanding the syntax of the Overpass query
language, while retaining the ability to handle arbitrarily complex
queries. Functions are also provided to enable recursive searching
between different kinds of OSM data (for example, to find all lines
which intersect a given point). The package is faster than current
alternatives for importing OSM data into R and is the only one
compatible with `sf`.

# References

# Google Summer of Code, 2017: Animint

Summary of the __Animated Interactive Plots (animint)__ project under the mentorship of _Toby Dylan Hocking_ and _Carson Sievert_ for the R project for statistical computing.

## Abstract
`animint` package in R allows animated data visualization which is a useful tool for obtaining an intuitive understanding of patterns in multivariate data sets used extensively in data related environments. It uses the `ggplot2` syntax and is based on the widely accepted _Grammar of Graphics_. This aim of the project was to build upon the already existing `animint` package by adding new useful features all the while making it more portable and developer friendly.

## Introduction
The motivation for this GSoC project stemmed from the fact that the `animint` package depends on the `ggplot2` package. This in itself is not a problem but it has been the root cause of many `animint` issues encountered by users in the past (`animint` issues [#176](https://github.com/tdhock/animint/issues/176), [#173](https://github.com/tdhock/animint/issues/173), [#104](https://github.com/tdhock/animint/issues/104)). To handle these frequent recurring issues, we needed to make some additions to the `ggplot2` source code, but our changes were not accepted by the maintainers of the package (`ggplot2` PR [#1649](https://github.com/tidyverse/ggplot2/pull/1649)). So it was decided that since we cannot make the changes to the original code and depending on a fork also creates problems, the best way to go would be to have our own fork edited specially for `animint` functionality. The fork would then be renamed to avoid issues where users had issues using the CRAN version of `ggplot2`. This idea led us to create two new packages: `animint2` - a successor to `animint`, and `ggplot2Animint` - `animint2` specific version of `ggplot2`.

## Frist Evaluation

# Google Summer of Code, 2017: Animint

Summary of the __Animated Interactive Plots (animint)__ project under the mentorship of _Toby Dylan Hocking_ and _Carson Sievert_ for the R project for statistical computing.

### Abstract
`animint` package in R allows animated data visualization which is a useful tool for obtaining an intuitive understanding of patterns in multivariate data sets used extensively in data related environments. It uses the `ggplot2` syntax and is based on the widely accepted _Grammar of Graphics_. This aim of the project was to build upon the already existing `animint` package by adding new useful features all the while making it more portable and developer friendly.

### Introduction
The motivation for this GSoC project stemmed from the fact that the `animint` package depends on the `ggplot2` package. This in itself is not a problem but it has been the root cause of many `animint` issues encountered by users in the past (`animint` issues [#176](https://github.com/tdhock/animint/issues/176), [#173](https://github.com/tdhock/animint/issues/173), [#104](https://github.com/tdhock/animint/issues/104)). To handle these frequent recurring issues, we needed to make some additions to the `ggplot2` source code, but our changes were not accepted by the maintainers of the package (`ggplot2` PR [#1649](https://github.com/tidyverse/ggplot2/pull/1649)). So it was decided that since we cannot make the changes to the original code and depending on a fork also creates problems, the best way to go would be to have our own fork edited specially for `animint` functionality. The fork would then be renamed to avoid issues where users had issues using the CRAN version of `ggplot2`. This idea led us to create two new packages: `animint2` - a successor to `animint`, and `ggplot2Animint` - `animint2` specific version of `ggplot2`.

### First Evaluation
For the major part of the first coding period, the main aim was to clean up the `animint` internals in `animint2` and try to skim off any redundant code ([Refactor Code](https://github.com/tdhock/animint2/pull/4)). The original implementation used a mutable meta environment which made the internals complicated and it was hard to contain errors. This was cleaned up to a large extent with separate new functions including appropriate documentation which helped later on in the project.

### Second Evaluation
After completing the refactoring part, we needed to solve the `ggplot2` dependency issue which was the top on our priority list. After much effort and deliberation, it was concluded that the newest version of `ggplot2`, version 2.2.1, was giving us an error which was hard and cumbersome to trace. Since our requirements were fulfilled by the `ggplot2` v2.1.0 already, we decided to fall back on that. It saved us some needless effort and time. The [validate-params branch in the `ggplot2` fork](https://github.com/tidyverse/ggplot2/compare/master...faizan-khan-iit:validate-params) was edited to remove the `ggplot2` dependency and the new package was named `ggplot2Animint`. This package supported the change in syntax proposed for later stages in the project.

### Final Evaluation

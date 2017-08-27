# Google Summer of Code, 2017: Animint

Summary of the __Animated Interactive Plots (animint)__ project under the _R project for statistical computing_.

##### Student: [Faizan Uddin Fahad Khan](https://github.com/faizan-khan-iit)

##### Mentors: [Toby Dylan Hocking](https://github.com/tdhock) and [Carson Sievert](https://github.com/cpsievert)

### Abstract
`animint` package in R allows animated data visualization which is a useful tool for obtaining an intuitive understanding of patterns in multivariate data sets used extensively in data related environments. It uses the `ggplot2` syntax and is based on the widely accepted _Grammar of Graphics_. This aim of the project was to build upon the already existing `animint` package by adding new useful features all the while making it more portable and developer friendly.

### Introduction
The motivation for this GSoC project stemmed from the fact that the `animint` package depends on the `ggplot2` package. This in itself is not a problem but it has been the root cause of many `animint` issues encountered by users in the past (`animint` issues [#176](https://github.com/tdhock/animint/issues/176), [#173](https://github.com/tdhock/animint/issues/173), [#104](https://github.com/tdhock/animint/issues/104)). To handle these frequent recurring issues, we needed to make some additions to the `ggplot2` source code, but our changes were not accepted by the maintainers of the package (`ggplot2` PR [#1649](https://github.com/tidyverse/ggplot2/pull/1649)). So it was decided that since we cannot make the changes to the original code and depending on a fork also creates problems, the best way to go would be to have our own fork edited specially for `animint` functionality. The fork would then be renamed to avoid issues where users had issues using the CRAN version of `ggplot2`. This idea led us to create two new packages: `animint2` - a successor to `animint`, and `ggplot2Animint` - `animint2` specific version of `ggplot2`.

### First Evaluation
For the major part of the first coding period, the main aim was to clean up the `animint` internals in `animint2` and try to skim off any redundant code ([Refactor Code](https://github.com/tdhock/animint2/pull/4)). The original implementation used a mutable meta environment which made the internals complicated and it was hard to contain errors. This was cleaned up to a large extent with separate new functions including appropriate documentation which helped later on in the project.

### Second Evaluation
After completing the refactoring part, we needed to solve the `ggplot2` dependency issue which was the top on our priority list. After much effort and deliberation, it was concluded that the newest version of `ggplot2`, version 2.2.1, was giving us an error which was hard and cumbersome to trace. Since our requirements were fulfilled by the `ggplot2` v2.1.0 already, we decided to fall back on that. It saved us some needless effort and a lot of time. The [validate-params branch in the `ggplot2` fork](https://github.com/tidyverse/ggplot2/compare/master...faizan-khan-iit:validate-params) was edited to remove the `ggplot2` dependency and the new package was named `ggplot2Animint`. This package supported the change in syntax proposed for later stages in the project.

### Final Evaluation
After removing the dependency on `ggplot2`, the first task was to adapt the `animint2` code to the new syntax. The two major features of the `animint` package: `showSelected` and `clickSelects` were to be passed as params, instead of the older convention where they were bundled with the other `aesthetics` of the layer.

```
# Older syntax
geom_point(aes(xVar, yVar, 
  clickSelects=clickVar,
  showSelected=showVar, showSelected2=showVar2,
  showSelected.variable=selector.name,
  showSelected.value=selector.value,
))

# New syntax
geom_point(aes(xVar, yVar), 
  clickSelects="clickVar", 
  showSelected=c("showVar", "showVar2", selector.name="selector.value"))
```

So all the tests and examples in the package had to be rewritten for the code to work. Getting the code to work proved to be  suprisingly (and notoriously) more complicated than imagined because the `ggplot2` code apparently uses environments to reference plots. As such, the `animint` code needed to either enforce the code to make deep copies of the plots to be rendered (which needed excess resources and computation) or needed a workaround. We preferred the workaround to keep only the original mapping for our purposes instead of the entire plot. After the CI build was passing, the PR was merged to support the new syntax. The package also depends on the `ggplot2Animint` package instead of the the CRAN `ggplot2`, so further changes to `ggplot2` won't require any changes to the `animint2` codebase.

### Link to commits
The links to all the commits ar given below:
1. Refactor Code: https://github.com/tdhock/animint2/pull/4/commits
2. `ggplot2` dependency and new syntax: https://github.com/tdhock/animint2/commits/dev?author=faizan-khan-iit
3. `ggplot2Animint`: https://github.com/faizan-khan-iit/ggplot2/commits/validate-params?author=faizan-khan-iit

### Future Work
Currently the aim is to introduce a `data.table` dependency in `animint2` for performance and stability improvements. The discussion can be [tracked here](https://github.com/tdhock/animint2/issues/2). Another idea for further improvement would be to port the geom specific code from `animint2` to `ggplot2Animint`.

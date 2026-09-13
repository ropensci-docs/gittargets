# Changelog

## gittargets 0.0.7.9000 (development)

## gittargets 0.0.7

CRAN release: 2023-12-04

- Migrate tests to `targets` \>= 1.3.2.9004 progress statuses
  (“completed” instead of “built”, “dispatched” instead of “started”).

## gittargets 0.0.6

CRAN release: 2023-02-09

- Import [`callr::r()`](https://callr.r-lib.org/reference/r.html).

## gittargets 0.0.5

CRAN release: 2022-09-06

- Use [`processx::run()`](http://processx.r-lib.org/reference/run.md)
  instead of [`system2()`](https://rdrr.io/r/base/system2.html) in
  [`tar_git_ok()`](https://docs.ropensci.org/gittargets/reference/tar_git_ok.md)
  and set `HOME` to `USERPROFILE` on Windows
  ([\#12](https://github.com/ropensci/gittargets/issues/12),
  [@psychelzh](https://github.com/psychelzh)).
- Handle errors invoking Git to get global user name and email.

## gittargets 0.0.4

CRAN release: 2022-08-05

- Compatibility with {targets} 0.13.0.

## gittargets 0.0.3

CRAN release: 2022-02-12

- Fix an example for CRAN.

## gittargets 0.0.2

- Hard reset after checkout in
  [`tar_git_checkout()`](https://docs.ropensci.org/gittargets/reference/tar_git_checkout.md)
  in order to recover potentially deleted files
  ([\#11](https://github.com/ropensci/gittargets/issues/11)).

## gittargets 0.0.1

CRAN release: 2022-01-13

- Join rOpenSci.
- Rewrite README to motivate the use case.
- Remove workflow diagram.
- Simplify snapshot model diagram.
- Fix the documentation of the `ref` argument of
  [`tar_git_checkout()`](https://docs.ropensci.org/gittargets/reference/tar_git_checkout.md).
- Add a section to the `git.Rmd` vignette on code merges.
- Allow command line Git tests to run locally on Windows.
- First version.

# Status of the targets (Git)

Show which targets are outdated.

## Usage

``` r
tar_git_status_targets(
  script = targets::tar_config_get("script"),
  store = targets::tar_config_get("store"),
  reporter = targets::tar_config_get("reporter_outdated"),
  envir = parent.frame(),
  callr_function = callr::r,
  callr_arguments = NULL
)
```

## Arguments

- script:

  Character of length 1, path to the target script file. Defaults to
  `tar_config_get("script")`, which in turn defaults to `_targets.R`.
  When you set this argument, the value of `tar_config_get("script")` is
  temporarily changed for the current function call. See
  [`tar_script()`](https://docs.ropensci.org/targets/reference/tar_script.html),
  [`tar_config_get()`](https://docs.ropensci.org/targets/reference/tar_config_get.html),
  and
  [`tar_config_set()`](https://docs.ropensci.org/targets/reference/tar_config_set.html)
  for details about the target script file and how to set it
  persistently for a project.

- store:

  Character of length 1, path to the `targets` data store. Defaults to
  `tar_config_get("store")`, which in turn defaults to `_targets/`. When
  you set this argument, the value of `tar_config_get("store")` is
  temporarily changed for the current function call. See
  [`tar_config_get()`](https://docs.ropensci.org/targets/reference/tar_config_get.html)
  and
  [`tar_config_set()`](https://docs.ropensci.org/targets/reference/tar_config_set.html)
  for details about how to set the data store path persistently for a
  project.

- reporter:

  Character of length 1, name of the reporter to user. Controls how
  messages are printed as targets are checked. Choices:

  - `"silent"`: print nothing.

  - `"forecast"`: print running totals of the checked and outdated
    targets found so far.

- envir:

  An environment, where to run the target R script (default:
  `_targets.R`) if `callr_function` is `NULL`. Ignored if
  `callr_function` is anything other than `NULL`. `callr_function`
  should only be `NULL` for debugging and testing purposes, not for
  serious runs of a pipeline, etc.

  The `envir` argument of
  [`tar_make()`](https://docs.ropensci.org/targets/reference/tar_make.html)
  and related functions always overrides the current value of
  `tar_option_get("envir")` in the current R session just before running
  the target script file, so whenever you need to set an alternative
  `envir`, you should always set it with
  [`tar_option_set()`](https://docs.ropensci.org/targets/reference/tar_option_set.html)
  from within the target script file. In other words, if you call
  `tar_option_set(envir = envir1)` in an interactive session and then
  `tar_make(envir = envir2, callr_function = NULL)`, then `envir2` will
  be used.

- callr_function:

  A function from `callr` to start a fresh clean R process to do the
  work. Set to `NULL` to run in the current session instead of an
  external process (but restart your R session just before you do in
  order to clear debris out of the global environment). `callr_function`
  needs to be `NULL` for interactive debugging, e.g.
  `tar_option_set(debug = "your_target")`. However, `callr_function`
  should not be `NULL` for serious reproducible work.

- callr_arguments:

  A list of arguments to `callr_function`.

## Value

A `tibble` with the names of outdated targets.

## Details

This function has prettier output than
[`targets::tar_outdated()`](https://docs.ropensci.org/targets/reference/tar_outdated.html),
and it mainly serves
[`tar_git_status()`](https://docs.ropensci.org/gittargets/reference/tar_git_status.md).

## See also

Other git:
[`tar_git_checkout()`](https://docs.ropensci.org/gittargets/reference/tar_git_checkout.md),
[`tar_git_init()`](https://docs.ropensci.org/gittargets/reference/tar_git_init.md),
[`tar_git_log()`](https://docs.ropensci.org/gittargets/reference/tar_git_log.md),
[`tar_git_ok()`](https://docs.ropensci.org/gittargets/reference/tar_git_ok.md),
[`tar_git_snapshot()`](https://docs.ropensci.org/gittargets/reference/tar_git_snapshot.md),
[`tar_git_status_code()`](https://docs.ropensci.org/gittargets/reference/tar_git_status_code.md),
[`tar_git_status_data()`](https://docs.ropensci.org/gittargets/reference/tar_git_status_data.md),
[`tar_git_status()`](https://docs.ropensci.org/gittargets/reference/tar_git_status.md)

## Examples

``` r
targets::tar_dir({ # Containing code does not modify the user's file space.
targets::tar_script(tar_target(data, 1))
targets::tar_make()
list.files("_targets", all.files = TRUE)
tar_git_status_targets()
})
#> + data dispatched
#> ✔ data completed [1ms, 50 B]
#> ✔ ended pipeline [113ms, 1 completed, 0 skipped]
#> Warning message:
#> package ‘targets’ was built under R version 4.6.1 
#> Warning message:
#> package ‘targets’ was built under R version 4.6.1 
#> 
#> # A tibble: 0 × 1
#> # ℹ 1 variable: outdated <chr>
```

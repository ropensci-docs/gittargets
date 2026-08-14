# Data snapshot menu (Git)

Check the project status and show an interactive menu for
[`tar_git_snapshot()`](https://docs.ropensci.org/gittargets/reference/tar_git_snapshot.md).

## Usage

``` r
tar_git_snapshot_menu(
  commit,
  message,
  code,
  script,
  store,
  stash_gitignore,
  reporter,
  envir,
  callr_function,
  callr_arguments
)
```

## Arguments

- commit:

  Character of length 1, Git SHA1 hash of the code commit that will
  correspond to the data snapshot (if created).

- message:

  Optional Git commit message of the data snapshot. If `NULL`, then the
  message is the Git commit message of the matching code commit.

- code:

  Character of length 1, directory path to the code repository, usually
  the root of the `targets` project.

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

  Character of length 1, path to the data store of the pipeline. If
  `NULL`, the `store` setting is left unchanged in the YAML
  configuration file (default: `_targets.yaml`). Usually, the data store
  lives at `_targets`. Set `store` to a custom directory to specify a
  path other than `_targets/`. The path need not exist before the
  pipeline begins, and it need not end with "\_targets", but it must be
  writeable. For optimal performance, choose a storage location with
  fast read/write access. If the argument `NULL`, the setting is not
  modified. Use
  [`tar_config_unset()`](https://docs.ropensci.org/targets/reference/tar_config_unset.html)
  to delete a setting.

- stash_gitignore:

  Logical of length 1, whether to temporarily stash the `.gitignore`
  file of the data store. See the "Stashing .gitignore" section for
  details.

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

Integer of length 1: `2L` if the user agrees to snapshot, `1L` if the
user declines.

## Examples

``` r
# See the examples of tar_git_snapshot().
```

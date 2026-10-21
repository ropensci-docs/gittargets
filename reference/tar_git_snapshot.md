# Snapshot the data repository (Git).

Snapshot the Git data repository of a `targets` project.

## Usage

``` r
tar_git_snapshot(
  message = NULL,
  ref = "HEAD",
  code = getwd(),
  script = targets::tar_config_get("script"),
  store = targets::tar_config_get("store"),
  stash_gitignore = TRUE,
  reporter = targets::tar_config_get("reporter_outdated"),
  envir = parent.frame(),
  callr_function = callr::r,
  callr_arguments = NULL,
  status = interactive(),
  force = FALSE,
  pack_refs = TRUE,
  verbose = TRUE
)
```

## Arguments

- message:

  Optional Git commit message of the data snapshot. If `NULL`, then the
  message is the Git commit message of the matching code commit.

- ref:

  Character of length 1, reference (branch name, Git SHA1 hash, etc.) of
  the code commit that will map to the new data snapshot. Defaults to
  the commit checked out right now.

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

- status:

  Logical of length 1, whether to print the project status with
  [`tar_git_status()`](https://docs.ropensci.org/gittargets/reference/tar_git_status.md)
  and ask whether a snapshot should be created.

- force:

  Logical of length 1. Force checkout the data branch of an existing
  data snapshot of the current code commit?

- pack_refs:

  Logical of length 1, whether to run `git pack-refs --all` in the data
  store after taking the snapshot. Packing references improves
  efficiency when the number of snapshots is large. Learn more at
  <https://git-scm.com/docs/git-pack-refs>.

- verbose:

  Logical of length 1, whether to print R console messages confirming
  that a snapshot was created.

## Details

A Git-backed `gittargets` data snapshot is a special kind of Git commit.
Every data commit is part of a branch specific to the current code
commit. That way, when you switch branches or commits in the code,
[`tar_git_checkout()`](https://docs.ropensci.org/gittargets/reference/tar_git_checkout.md)
checks out the latest data snapshot that matches the code in your
workspace. That way, your targets can stay up to date even as you
transition among multiple branches.

## Stashing .gitignore

The `targets` package writes a `.gitignore` file to new data stores in
order to prevent accidental commits to the code Git repository.
Unfortunately, for `gittargets`, this automatic `.gitignore` file
interferes with proper data versioning. So by default, `gittargets`
temporarily stashes it to a hidden file called `.gittargets_gitignore`
inside the data store. If your R program crashes while the stash is
active, you can simply move it manually back to `.gitignore` or run
[`tar_git_status_data()`](https://docs.ropensci.org/gittargets/reference/tar_git_status_data.md)
to restore the stash automatically if no `.gitignore` already exists.

## See also

Other git:
[`tar_git_checkout()`](https://docs.ropensci.org/gittargets/reference/tar_git_checkout.md),
[`tar_git_init()`](https://docs.ropensci.org/gittargets/reference/tar_git_init.md),
[`tar_git_log()`](https://docs.ropensci.org/gittargets/reference/tar_git_log.md),
[`tar_git_ok()`](https://docs.ropensci.org/gittargets/reference/tar_git_ok.md),
[`tar_git_status_code()`](https://docs.ropensci.org/gittargets/reference/tar_git_status_code.md),
[`tar_git_status_data()`](https://docs.ropensci.org/gittargets/reference/tar_git_status_data.md),
[`tar_git_status_targets()`](https://docs.ropensci.org/gittargets/reference/tar_git_status_targets.md),
[`tar_git_status()`](https://docs.ropensci.org/gittargets/reference/tar_git_status.md)

## Examples

``` r
if (Sys.getenv("TAR_EXAMPLES") == "true" && tar_git_ok(verbose = FALSE)) {
targets::tar_dir({ # Containing code does not modify the user's filespace.
targets::tar_script(tar_target(data, 1))
targets::tar_make()
gert::git_init()
gert::git_add("_targets.R")
gert::git_commit("First commit")
tar_git_init()
tar_git_snapshot(status = FALSE)
})
}
```

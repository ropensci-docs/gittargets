# Data snapshots of a code branch (Git)

Show all the data snapshots of a code branch.

## Usage

``` r
tar_git_log(
  code = getwd(),
  store = targets::tar_config_get("store"),
  branch = gert::git_branch(repo = code),
  max = 100
)
```

## Arguments

- code:

  Character of length 1, directory path to the code repository, usually
  the root of the `targets` project.

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

- branch:

  Character of length 1, name of the code repository branch to query.
  Defaults to the currently checked-out code branch.

- max:

  Positive numeric of length 1, maximum number of code commits to
  inspect for the given branch.

## Value

A data frame of information about data snapshots and code commits.

## Details

By design, `tar_git_log()` only queries a single code branch at a time.
This allows `tar_git_log()` to report more detailed information about
the snapshots of the given code branch. To query all data snapshots over
all branches, simply run
`gert::git_branch_list(local = TRUE, repo = "_targets")`. The valid
snapshots show `"code=<SHA1>"` in the `name` column, where `<SHA1>` is
the Git commit hash of the code commit corresponding to the data
snapshot.

## See also

Other git:
[`tar_git_checkout()`](https://docs.ropensci.org/gittargets/reference/tar_git_checkout.md),
[`tar_git_init()`](https://docs.ropensci.org/gittargets/reference/tar_git_init.md),
[`tar_git_ok()`](https://docs.ropensci.org/gittargets/reference/tar_git_ok.md),
[`tar_git_snapshot()`](https://docs.ropensci.org/gittargets/reference/tar_git_snapshot.md),
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
tar_git_snapshot(status = FALSE, verbose = FALSE)
tar_git_log()
})
}
```

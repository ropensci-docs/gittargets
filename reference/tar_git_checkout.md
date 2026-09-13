# Check out a snapshot of the data (Git)

Check out a snapshot of the data associated with a particular code
commit (default: `HEAD`).

## Usage

``` r
tar_git_checkout(
  ref = "HEAD",
  code = getwd(),
  store = targets::tar_config_get("store"),
  force = FALSE,
  verbose = TRUE
)
```

## Arguments

- ref:

  Character of length 1. SHA1 hash, branch name, or other reference in
  the code repository that points to a code commit. (You can also
  identify the code commit by supplying a data branch of the form
  `code=<SHA1>`.) Defaults to `"HEAD"`, which points to the currently
  checked out code commit.

  Once the desired code commit is identified,
  [`tar_git_snapshot()`](https://docs.ropensci.org/gittargets/reference/tar_git_snapshot.md)
  checks out the latest corresponding data snapshot. There may be
  earlier data snapshots corresponding to this code commit, but
  [`tar_git_snapshot()`](https://docs.ropensci.org/gittargets/reference/tar_git_snapshot.md)
  only checks out the latest one. To check out an earlier superseded
  data snapshot, you will need to manually use command line Git in the
  data repository.

  If
  [`tar_git_snapshot()`](https://docs.ropensci.org/gittargets/reference/tar_git_snapshot.md)
  cannot find a data snapshot for the desired code commit, then it will
  throw an error. For a list of commits in the current code branch that
  have available data snapshots, see the `commit_code` column of the
  output of
  [`tar_git_log()`](https://docs.ropensci.org/gittargets/reference/tar_git_log.md).

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

- force:

  ignore conflicts and overwrite modified files

- verbose:

  Logical of length 1, whether to print R console messages confirming
  that a snapshot was created.

## Value

Nothing (invisibly).

## See also

Other git:
[`tar_git_init()`](https://docs.ropensci.org/gittargets/reference/tar_git_init.md),
[`tar_git_log()`](https://docs.ropensci.org/gittargets/reference/tar_git_log.md),
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
# Work on an initial branch.
targets::tar_script(tar_target(data, "old_data"))
targets::tar_make()
targets::tar_read(data) # "old_data"
gert::git_init()
gert::git_add("_targets.R")
gert::git_commit("First commit")
gert::git_branch_create("old_branch")
tar_git_init()
# Work on a new branch.
tar_git_snapshot(status = FALSE, verbose = FALSE)
targets::tar_script(tar_target(data, "new_data"))
targets::tar_make()
targets::tar_read(data) # "new_data"
gert::git_branch_create("new_branch")
gert::git_add("_targets.R")
gert::git_commit("Second commit")
tar_git_snapshot(status = FALSE, verbose = FALSE)
# Go back to the old branch.
gert::git_branch_checkout("old_branch")
# The target is out of date because we only reverted the code.
targets::tar_outdated()
# But tar_git_checkout() lets us restore the old version of the data!
tar_git_checkout()
targets::tar_read(data) # "old_data"
# Now, the target is up to date! And we did not even have to rerun it!
targets::tar_outdated()
})
}
```

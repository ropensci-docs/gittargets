# Status of the data repository (Git)

Show the Git status of the data repository.

## Usage

``` r
tar_git_status_data(
  store = targets::tar_config_get("store"),
  stash_gitignore = TRUE
)
```

## Arguments

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

## Value

If the data repository exists, the return value is the data frame
produced by `gert::git_status(repo = store)`. If the data store has no
Git repository, then the return value is `NULL`.

## Stashing .gitignore

The `targets` package writes a `.gitignore` file to new data stores in
order to prevent accidental commits to the code Git repository.
Unfortunately, for `gittargets`, this automatic `.gitignore` file
interferes with proper data versioning. So by default, `gittargets`
temporarily stashes it to a hidden file called `.gittargets_gitignore`
inside the data store. If your R program crashes while the stash is
active, you can simply move it manually back to `.gitignore` or run
`tar_git_status_data()` to restore the stash automatically if no
`.gitignore` already exists.

## See also

Other git:
[`tar_git_checkout()`](https://docs.ropensci.org/gittargets/reference/tar_git_checkout.md),
[`tar_git_init()`](https://docs.ropensci.org/gittargets/reference/tar_git_init.md),
[`tar_git_log()`](https://docs.ropensci.org/gittargets/reference/tar_git_log.md),
[`tar_git_ok()`](https://docs.ropensci.org/gittargets/reference/tar_git_ok.md),
[`tar_git_snapshot()`](https://docs.ropensci.org/gittargets/reference/tar_git_snapshot.md),
[`tar_git_status_code()`](https://docs.ropensci.org/gittargets/reference/tar_git_status_code.md),
[`tar_git_status_targets()`](https://docs.ropensci.org/gittargets/reference/tar_git_status_targets.md),
[`tar_git_status()`](https://docs.ropensci.org/gittargets/reference/tar_git_status.md)

## Examples

``` r
if (Sys.getenv("TAR_EXAMPLES") == "true" && tar_git_ok(verbose = FALSE)) {
targets::tar_dir({ # Containing code does not modify the user's file space.
targets::tar_script(tar_target(data, 1))
targets::tar_make()
list.files("_targets", all.files = TRUE)
gert::git_init()
tar_git_init()
tar_git_status_data()
})
}
```

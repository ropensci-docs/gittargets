# Status of the code repository (Git)

Show the Git status of the code repository.

## Usage

``` r
tar_git_status_code(code = getwd())
```

## Arguments

- code:

  Character of length 1, directory path to the code repository, usually
  the root of the `targets` project.

## Value

If the code repository exists, the return value is the data frame
produced by `gert::git_status(repo = code)`. If the code has no Git
repository, then the return value is `NULL`.

## See also

Other git:
[`tar_git_checkout()`](https://docs.ropensci.org/gittargets/reference/tar_git_checkout.md),
[`tar_git_init()`](https://docs.ropensci.org/gittargets/reference/tar_git_init.md),
[`tar_git_log()`](https://docs.ropensci.org/gittargets/reference/tar_git_log.md),
[`tar_git_ok()`](https://docs.ropensci.org/gittargets/reference/tar_git_ok.md),
[`tar_git_snapshot()`](https://docs.ropensci.org/gittargets/reference/tar_git_snapshot.md),
[`tar_git_status_data()`](https://docs.ropensci.org/gittargets/reference/tar_git_status_data.md),
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
tar_git_status_code()
})
}
```

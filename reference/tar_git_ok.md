# Check Git

Check if Git is installed and if `user.name` and `user.email` are
configured globally.

## Usage

``` r
tar_git_ok(verbose = TRUE)
```

## Arguments

- verbose:

  Whether to print messages to the console.

## Value

Logical of length 1, whether Git is installed and configured correctly.

## Details

You can install Git from <https://git-scm.com/downloads/> and configure
your identity using the instructions at
<https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup>.
You may find it convenient to run
[`gert::git_config_global()`](https://docs.ropensci.org/gert/reference/git_config.html)
with `name` equal to `user.name` and `user.email`.

## See also

Other git:
[`tar_git_checkout()`](https://docs.ropensci.org/gittargets/reference/tar_git_checkout.md),
[`tar_git_init()`](https://docs.ropensci.org/gittargets/reference/tar_git_init.md),
[`tar_git_log()`](https://docs.ropensci.org/gittargets/reference/tar_git_log.md),
[`tar_git_snapshot()`](https://docs.ropensci.org/gittargets/reference/tar_git_snapshot.md),
[`tar_git_status_code()`](https://docs.ropensci.org/gittargets/reference/tar_git_status_code.md),
[`tar_git_status_data()`](https://docs.ropensci.org/gittargets/reference/tar_git_status_data.md),
[`tar_git_status_targets()`](https://docs.ropensci.org/gittargets/reference/tar_git_status_targets.md),
[`tar_git_status()`](https://docs.ropensci.org/gittargets/reference/tar_git_status.md)

## Examples

``` r
tar_git_ok()
#> ✔ Git binary: /usr/bin/git
#> ✖ Error getting Git global user name and email:
#> ✖ ! System command 'git' failed
#> [1] FALSE
```

# targets: Dynamic Function-Oriented Make-Like Declarative Pipelines for R

In computationally demanding data analysis pipelines, the `targets` R
package maintains an up-to-date set of results while skipping tasks that
do not need to rerun. This process increases speed and increases trust
in the final end product. However, it also overwrites old output with
new output, and past results disappear by default. To preserve
historical output, the `gittargets` package captures version-controlled
snapshots of the data store, and each snapshot links to the underlying
commit of the source code. That way, when the user rolls back the code
to a previous branch or commit, `gittargets` can recover the data
contemporaneous with that commit so that all targets remain up to date.

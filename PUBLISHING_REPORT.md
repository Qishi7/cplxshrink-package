# Repository organization and publication

## Official fork

Repository: https://github.com/Qishi7/cplxshrink-package
Parent: https://github.com/chenghanyustats/cplxshrink-package
Development branch: feature/posterior-interface-v02

GitHub confirms this repository is a fork of the upstream manuscript package.
main is retained at d985957c4254593ae2cc5c449d6dc3d108dd1b5b; development is
on the separate feature branch. No upstream write or upstream pull request is made.

The feature source is migrated from the previously validated development tree,
including the cleaned package-focused README. Package code, protected sampler
files and reference fixtures are unchanged. Only destination metadata and review
provenance are updated. Historical local validation reports and previous publication
logs are retained as dated evidence, not claims about the current repository location.

## Separate cplxrv project

Qishi7/cplxrv is a separate existing project. The owner requested restoring its
original contents using a new commit, preserving all intervening history.
The target original tree is commit 7ef7f9e3decb6294e6774d08d9444768f6ad9f58.
Its previous replacement/development commits remain recoverable from history.
The new restoration commit will be recorded here after it is pushed and verified.

The earlier publication record is retained in
review/history/previous-cplxrv-publication.md.

## Validation

Earlier local runs passed 444 expectations and standard R CMD check with zero
errors/warnings/notes. Fresh verification for this fork is recorded under
review/logs/fork-migration/. No new algorithm changes are introduced.
The owner accepts local validation; cross-platform CI is not claimed as executed.
The workflow remains available for future use. Upload commits skip CI.

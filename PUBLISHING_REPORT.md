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
Restoration was pushed as commit
43e147b9d6eb71985cfa71cb25ad7d4ee320213d on cplxrv main. Its tree is
1d1068953da8db030e9c8252cd7ca5337bdab9ba, exactly matching the original
7ef7f9e3decb6294e6774d08d9444768f6ad9f58 tree. A git diff against the original
returned exit 0. The intervening replacement and README commits remain ancestors.
No force-push or history rewrite was used.

The earlier publication record is retained in
review/history/previous-cplxrv-publication.md.

## Validation

Earlier local runs passed 444 expectations and standard R CMD check with zero
errors/warnings/notes. Fresh verification for this fork is recorded under
review/logs/fork-migration/. No new algorithm changes are introduced.
The owner accepts local validation; cross-platform CI is not claimed as executed.
The workflow remains available for future use. Upload commits skip CI.

## Migration verification

Tested fork source commit: 0bacca0dff5d5a199b1521380d115270819913a7.
The fork and development branch were pushed and independently verified through
GitHub metadata and git ls-remote. main remains at the frozen upstream commit.

The fresh testthat run passed 444 expectations, with zero failures, errors,
warnings or skips. The first build/check included an in-progress compiler
.o.tmp file because local compilation and source packaging overlapped; it
produced one source-file warning. That log is preserved as initial-check.log.
The final build and check use a clean git archive export in a separate directory,
with no compilation intermediates. Final results are in
review/logs/fork-migration/{build,check}.log. No engine changes were needed.

Final clean-export R CMD build succeeded. Standard R CMD check: Status OK,
0 errors, 0 warnings, 0 notes; examples, original smoke script, testthat and
PDF manual all passed.

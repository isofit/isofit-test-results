# isofit-test-results

Automatically-generated test and example results for [ISOFIT](https://github.com/isofit/isofit).

This repository is a results store, not source code. GitHub Actions run a fixed
set of ISOFIT examples, generate plots and resource-usage reports, and commit the
outputs back here so they can be browsed and compared across ISOFIT revisions.

## How it works

- ISOFIT's own CI runs the example test cases, renders figures and resource
  reports with `isoplots`, and commits the outputs directly into this repository —
  into [`dev/`](dev) on `main`, or into a per-PR branch for pull-request builds.
- The [`Rebase`](.github/workflows/rebase.yml) workflow keeps the per-PR branches
  rebased on top of `main` (using `-Xtheirs`) whenever `main` is updated.
- The [`Cleanup`](.github/workflows/cleanup.yml) workflow runs weekly and closes
  result PRs (and deletes their branches) once the ISOFIT PR they track is merged
  or closed.

## Layout

```
dev/                      # results built from ISOFIT's dev branch
  lake_mary/              # Lake Mary single-pixel retrieval (default + bgrfl)
  SeaBASS_prism_001/      # SeaBASS PRISM example
  imagecube_small/        # small image-cube spectra
.github/workflows/        # Rebase + Cleanup automation for this repo
```

Each test-case folder contains the generated artifacts, typically:

- `*.png` — retrieval / spectra plots
- `*.log` — ISOFIT run log
- `*.resources.jsonl` / `*.resources.html` / `*.resources.png` — resource-usage
  (timing/memory) data and report

## Branches

- `main` tracks results built from ISOFIT's `dev` branch.
- Numbered branches (e.g. `1042`) correspond to ISOFIT pull requests. Their
  results are rebuilt and kept rebased on `main` so a PR's output can be diffed
  against the `dev` baseline.

## Adding a new test case

Test cases are defined and run by ISOFIT's CI, which commits their output here.
To add one, add the example to ISOFIT's test workflow so it generates the plots
and resource report and commits them into a new `dev/<name>/` subfolder. No
configuration is needed in this repository.

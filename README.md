# isofit-test-results

Automatically-generated test and example results for [ISOFIT](https://github.com/isofit/isofit).

This repository is a results store, not source code. GitHub Actions run a fixed
set of ISOFIT examples, generate plots and resource-usage reports, and commit the
outputs back here so they can be browsed and compared across ISOFIT revisions.

## How it works

- Each test case is a reusable workflow (`results_*.yml`) that executes an ISOFIT
  example, renders figures and a resource report with `isoplots`, and uploads the
  output as a build artifact.
- The `upload` job collects every artifact and commits the results into
  [`dev/`](dev), one subfolder per test case.
- The [`Rebase`](.github/workflows/rebase.yml) workflow keeps the per-PR branches
  rebased on top of `main`.

## Layout

```
dev/                      # results built from ISOFIT's dev branch
  lake_mary/              # Lake Mary single-pixel retrieval (default + bgrfl)
  SeaBASS_prism_001/      # SeaBASS PRISM example
  imagecube_small/        # small image-cube spectra
.github/workflows/        # CI that produces the results above
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

1. Copy the template workflow (`results_template.yml`) to a new
   `results_<name>.yml`, and fill in the commands that generate the files you want
   uploaded into the `results/` directory.
2. Register it in [`dev.yml`](.github/workflows/dev.yml): add a job that calls your
   new reusable workflow and list that job under the `upload` job's `needs`.

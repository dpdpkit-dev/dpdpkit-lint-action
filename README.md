# dpdp-lint

A GitHub Action that fails your CI when your `dpdpkit.yaml` is missing something the DPDP Act needs you to
configure. It installs `dpdpkit-core` and runs `dpdpkit lint`. It needs no database and no network access beyond
installing the package.

It checks that:

- the **notice** is configured, and passes the automated notice checks (required links, itemised data and
  purposes, translations, contact details; placeholder text is a warning);
- every **purpose has a retention rule** (`retention.period_days` or `retention.third_schedule_class`).

It checks that the configuration is complete. It does not decide whether your purposes, wording or retention
periods are lawful; that is for you and your counsel.

## Use

```yaml
name: dpdp-lint
on: [pull_request]

permissions:
  contents: read

jobs:
  dpdp-lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v7
      - uses: dpdpkit-dev/dpdpkit-lint-action@v1
        with:
          config: dpdpkit.yaml
```

Findings appear as annotations on the pull request, pointing at the file.

## Inputs

| Input | Default | Meaning |
| --- | --- | --- |
| `config` | `dpdpkit.yaml` | Paths to lint, separated by spaces or newlines. |
| `strict` | `false` | Also fail on warnings. |
| `version` | latest | `dpdpkit-core` version to install. Pin it for reproducible builds. |
| `python-version` | `3.12` | Python used to run the linter. |

## Run it locally

```
pip install dpdpkit-core
dpdpkit lint dpdpkit.yaml        # or: dpdp-lint dpdpkit.yaml
```

Exit code 1 means a failure (or a warning with `--strict`).

## Licence

Apache-2.0 (see `LICENSE`).

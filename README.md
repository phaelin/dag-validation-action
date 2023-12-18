# DAG Validation Action

This GitHub action runs tests against Airflow DAGs to validate them before deployment. It is useful for catching errors early in the development cycle on pull requests.

When committed code changes Python files containing Airflow DAGs, this action will:

- Checkout the code from the GitHub repository
- Install any requirements from requirements.txt
- Import all DAG files
- Run the Airflow test suite
- If any tests fail, the action will fail the build and prevent merging until issues are resolved.

This provides an extra layer of validation for Airflow DAGs beyond unit and integration tests. It helps catch configuration and import errors before code is deployed to production.

## Usage
To use this action, add a `.github/workflows/dag-validation.yml` file to your repository. The workflow should contain:

```yml
name: DAG Validation
on:
  push:
    paths:
      - 'dags/**'
    pull_request:
      branches:
        - main

jobs:
  linting:
    runs-on: ubuntu-latest
    steps:
      - name: Lint DAGs with Flake8
        uses: actions/dag-validation-action@main # TODO: update with repo value and version
        with:
          flake8: true
          dags: 'code/dags' # default: `dags`
      - name: Check code compliance with Black
        uses: actions/dag-validation-action@main
        with:
          black: true
  testing:
      - name: Test DAGs with PyTest
        uses: actions/dag-validation-action@main
        with:
          pytest: true
          testdir: 'code/tests' # default: `tests`
```


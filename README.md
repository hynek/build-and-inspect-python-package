# build-and-inspect-python-package

> **Warning**
> This is alpha software and may change before v1.

This action provides the following functionality for GitHub Actions users that are maintaining Python PyPI packages:

1. Build your package using PyPA's [*build*](https://pypi.org/project/build/),
1. upload the *wheel* and the source distribution (*SDist*) as artifacts so you can download and inspect them from the Summary view of a Actions run,
1. lint the wheel using [*check-wheel-contents*](https://pypi.org/project/check-wheel-contents/),
1. lint the PyPI README of both *wheel* and *SDist* using [*twine*](https://pypi.org/project/twine/),
1. print you the tree of both *SDist* and *wheel* in the CI output so you don't have to download the packages just to check the contents.

This way you can make package verification part of your CI.

> **Note**
> This action does **not** upload your package to PyPI.


## Usage

```yaml
jobs:
  check-package:
    name: Build & inspect our package.
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3
      - uses: hynek/build-and-inspect-python-package@v0.1
```

### Optional Inputs

- `path`: the location of the Python package to build (default: `.`).


## License

The scripts and documentation in this project are released under the [MIT License](LICENSE).

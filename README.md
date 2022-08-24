# build-and-inspect-python-package

This action provides the following functionality for GitHub Actions users that are maintaining Python packages and want to ensure in CI that the packages they will build are in good shape and remain so:

1. Builds your package using PyPA's [*build*](https://pypi.org/project/build/) (this works with any [PEP 517](https://peps.python.org/pep-0517/)-compatible build backend, including *Hatch*, *Flit*, *Setuptools*, *PDM*, or *Poetry*),
1. uploads the *wheel* and the source distribution (*SDist*) as GitHub Actions artifacts (**not** PyPI) so you can download and inspect them from the Summary view of a Actions run,
1. lints the wheel using [*check-wheel-contents*](https://pypi.org/project/check-wheel-contents/),
1. lints the PyPI README of both *wheel* and *SDist* using [*twine*](https://pypi.org/project/twine/),
1. prints the tree of both *SDist* and *wheel* in the CI output, so you don't have to download the packages to just check the contents,
1. prints and uploads the *SDist*'s `PKG-INFO` file (that is also the `METADATA` file in each wheel) as a GitHub Actions artifact,
1. upload the packages PyPI README, just as it would appear on the project's PyPI landing page as a GitHub Actions artifact (to level up your PyPI README game, check out [*hatch-fancy-pypi-readme*](https://github.com/hynek/hatch-fancy-pypi-readme)).

If you package an **application** as a Python package, this action is useful to double-check you're shipping everything you need, including all templates, translation files, et cetera.


## Usage

```yaml
jobs:
  check-package:
    name: Build & inspect our package.
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3
      - uses: hynek/build-and-inspect-python-package@v1
```

The action will run [*setup-python*](https://github.com/actions/setup-python) with Python **3.10** on its own.


### Inputs

- `path`: the location of the Python package to build (*optional*, default: `.`).


### Outputs

- `dist`: the location with the built packages.

  See for example how [*argon2-cffi-bindings*](https://github.com/hynek/argon2-cffi-bindings/blob/a9d295e577b271b1c7f6ca3929fe8b39ba8b689e/.github/workflows/ci.yml#L75-L85) uses this feature to check the built wheels don't break a dependency.


### Artifacts

After a successful run, you'll find multiple artifacts in the run's Summary view:

- **Packages**: the built packages.
- **Package Metadata**: the extracted packaging metadata (*hint*: it's formatted as an email).
- **PyPI README**: the extracted PyPI README, exactly how it would be used by PyPI as your project's landing page.
  [PEP 621](https://peps.python.org/pep-0621/) calls it `readme`, in classic *setuptools* it's `long_description`.

---

[Our CI](.github/workflows/ci.yml) uses all inputs and outputs, if you want to see them in action.

## License

The scripts and documentation in this project are released under the [MIT License](LICENSE).

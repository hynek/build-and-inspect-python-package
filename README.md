# build-and-inspect-python-package

This action provides the following functionality for GitHub Actions users that are maintaining Python packages and want to ensure in CI that the packages they build are in good shape and remain so:

**Builds your package** using PyPA's [*build*](https://pypi.org/project/build/) (this works with any [PEP 517](https://peps.python.org/pep-0517/)-compatible build backend, including *Hatch*, *Flit*, *Setuptools*, *PDM*, or *Poetry*).

Uploads the **built *wheel* and the source distribution (*SDist*) as GitHub Actions artifacts** (*not* PyPI), so you can download and inspect them from the Summary view of a run.

Lints the **wheel contents** using [*check-wheel-contents*](https://pypi.org/project/check-wheel-contents/).

Lints the **PyPI README** using [*twine*](https://pypi.org/project/twine/) and uploads it as an GitHub Actions artifact for further inspection.
To level up your PyPI README game, check out [*hatch-fancy-pypi-readme*](https://github.com/hynek/hatch-fancy-pypi-readme)!

Prints the **tree of both *SDist* and *wheel*** in the CI output, so you don't have to download the packages, if you just want to check the content list.

Prints and uploads the **packaging metadata** as a GitHub Actions artifact.

---

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

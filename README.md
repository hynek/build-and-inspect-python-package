# Build and Inspect Python Packages

This action provides the following functionality for GitHub Actions users that are maintaining Python packages:

**Builds your package** using PyPA's [*build*](https://pypi.org/project/build/) (this works with any [PEP 517](https://peps.python.org/pep-0517/)-compatible build backend, including Hatch, Flit, Setuptools, PDM, or Poetry).
[`SOURCE_DATE_EPOCH`](https://reproducible-builds.org/specs/source-date-epoch/) is set to the timestamp of the last commit, giving you reproducible builds with meaningful file timestamps.

Uploads the **built *wheel* and the source distribution (*SDist*) as GitHub Actions artifacts**, so you can download and inspect them from the Summary view of a run, or [**upload them to PyPI automatically**][automated] once the verification succeeds.

Lints the **wheel contents** using [*check-wheel-contents*](https://pypi.org/project/check-wheel-contents/).

Lints the **PyPI README** using [Twine](https://pypi.org/project/twine/) and uploads it as an GitHub Actions artifact for further inspection.
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
      - uses: actions/checkout@v4
      - uses: hynek/build-and-inspect-python-package@v1
```

> [!CAUTION]
> Internally, *build-and-inspect-python-package* uses [*actions/upload-artifact*](https://github.com/actions/upload-artifact) for storing the built artifacts that you can download with [*actions/download-artifact*](https://github.com/actions/download-artifact).
>
> Unfortunately, v4 of both [is incompatible](https://github.blog/changelog/2023-12-14-github-actions-artifacts-v4-is-now-generally-available/) with previous versions, so you have to make sure that your *download-artifact* version matches the version that *build-and-inspect-python-package* uses for uploading.
>
> If you're using `download-artifact@v3`, you have to use `build-and-inspect-python-package@v1`.
> If you're using `download-artifact@v4`, you have to use `build-and-inspect-python-package@v2`.


### Inputs

- `path`: the location of the Python package to build (*optional*, default: `.`).
- `skip-wheel`: Whether to skip building the wheel in addition to the source distribution.
  The only meaningful value is `'true'` (note the quotes – GitHub Actions only allow string inputs) and everything else is treated as falsey.
  This is useful if you need to build your wheels using advanced tools like [*cibuildwheel*](https://cibuildwheel.pypa.io/) anyway.
  (*optional*, default: `'false'`).


### Outputs

- `dist`: the location with the built packages.

  See for example how [*argon2-cffi-bindings*](https://github.com/hynek/argon2-cffi-bindings/blob/a9d295e577b271b1c7f6ca3929fe8b39ba8b689e/.github/workflows/ci.yml#L75-L85) uses this feature to check the built wheels don't break a dependency.


### Artifacts

After a successful run, you'll find multiple artifacts in the run's Summary view:

- **Packages**: The built packages.
  Perfect for [automated PyPI upload workflows][automated]!
- **Package Metadata**: the extracted packaging metadata (*hint*: it's formatted as an email).
- **PyPI README**: the extracted PyPI README, exactly how it would be used by PyPI as your project's landing page.
  [PEP 621](https://peps.python.org/pep-0621/) calls it `readme`, in classic *setuptools* it's `long_description`.

---

[Our CI](.github/workflows/) uses all inputs and outputs, if you want to see them in action.


## License

The scripts and documentation in this project are released under the [MIT License](LICENSE).

[automated]: https://github.com/python-attrs/attrs/blob/main/.github/workflows/pypi-package.yml

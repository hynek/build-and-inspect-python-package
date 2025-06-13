<p align="center">
  <img alt="build-and-inspect-python-package logo" width="250" src=".github/logo.png" />
  <br/>
  <em>Never upload a faulty Python package to PyPI again.</em>
</p>

*build-and-inspect-python-package* is a GitHub Action that provides the following functionality to Python package maintainers:

**Builds your package**[^backend].
[`SOURCE_DATE_EPOCH`](https://reproducible-builds.org/specs/source-date-epoch/) is set to the timestamp of the last commit, giving you reproducible builds with meaningful file timestamps.

[^backend]: Works with any [PEP 517](https://peps.python.org/pep-0517/)-compatible build backend. This includes Hatchling, Flit, Setuptools, PDM, and Poetry.

Uploads the **built *wheel* and the source distribution (*SDist*) as GitHub Actions artifacts**, so you can download and inspect them from the Summary view of a run, or [**upload them to PyPI automatically**][automated] once the verification succeeds.

Lints the **wheel contents** using [*check-wheel-contents*](https://pypi.org/project/check-wheel-contents/).

Lints the **PyPI README** using [Twine](https://pypi.org/project/twine/) and uploads it as an GitHub Actions artifact for further manual inspection.
To level up your PyPI README game, check out [*hatch-fancy-pypi-readme*](https://github.com/hynek/hatch-fancy-pypi-readme)!

Prints the **tree of both *SDist* and *wheel*** in the CI output, so you don’t have to download the packages, if you just want to check the content list.

Prints and uploads the **packaging metadata** as a GitHub Actions artifact.


## Popular Use Cases

### Build Once – Use Across Jobs

To increase the fidelity of your tests to what your users will experience, you can build and store your package as a first step, depend on the step in the remaining steps, and – instead of checking out the source tree – retrieve the built packages and run your tests against *that*.
For example, by unpacking the tests and config from the SDist and using `tox run --installpkg dist/*.whl ...` to run the tests against the built wheel without access to the package source code.

You can see this technique in action in [*structlog*’s CI](https://github.com/hynek/structlog/blob/main/.github/workflows/ci.yml).


### Automatic Uploading

You can use a workflow that builds your package and – depending on the CI event (push to main, new tag, new release, ...) – uses [PyPI’s trusted publisher feature](https://blog.pypi.org/posts/2023-04-20-introducing-trusted-publishers/) to upload it to [Test PyPI](https://test.pypi.org)[^unique], PyPI, or both.
This way you can continuously check how the package will look on PyPI.

*structlog* [uses](https://github.com/hynek/structlog/blob/main/.github/workflows/pypi-package.yml) this technique too:
It uploads every commit on `main` to [Test PyPI](https://test.pypi.org/project/structlog/#history) and whenever a [GitHub Release](https://github.com/hynek/structlog/releases) is created, also to the real PyPI.

[^unique]: Note, though, that a prerequisite for the Test PyPI workflow is that each of your commits builds with a unique version number.
  This is easily achievable using tools like [*setuptools-scm*](https://setuptools-scm.readthedocs.io/) or [*hatch-vcs*](https://github.com/ofek/hatch-vcs), but beyond the scope of this humble README.


### Define Python Version Matrix Based On Package Metadata

*build-and-inspect-python-package* extracts the Python versions your package supports from the trove classifiers in your package’s metadata and offers them as an action output.

That means that you can define your CI matrix based on the Python versions your package supports without duplicating the information between your package configuration and your CI configuration.


### Applications

If you package an **application** as a Python package, this action is useful to double-check you’re shipping everything you need, including all templates, translation files, et cetera.


## Usage

*build-and-inspect-python-package* only works on Linux runners:

```yaml
jobs:
  build-and-inspect-package:
    name: Build & inspect package.
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4
      - uses: hynek/build-and-inspect-python-package@v2
```

To also upload to PyPI:

```yaml
jobs:
  build-and-inspect-package:
    name: Build & inspect package.
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4
      - uses: hynek/build-and-inspect-python-package@v2


  upload-to-pypi:
    name: Upload package to PyPI
    needs: build-and-inspect-package
    runs-on: ubuntu-latest
    permissions:
      # IMPORTANT: this permission is mandatory for trusted publishing, but
      # should NOT be granted anywhere else!
      id-token: write

    steps:
      - name: Download built artifact to dist/
        uses: actions/download-artifact@v4
        with:
          name: Packages
          path: dist
      - uses: pypa/gh-action-pypi-publish@release/v1
```

> [!IMPORTANT]
> For security reasons, keep the job that has the `id-token: write` permission as short as possible.

---

If you’re using a VCS tag-based version extractor like [*setuptools-scm*] and need the built package to have the correct version, you must use *actions/checkout* with `fetch-depth: 0` – unless the latest commit _is_ the version tag.

> [!CAUTION]
> *build-and-inspect-python-package* uses [*actions/upload-artifact*](https://github.com/actions/upload-artifact) for storing the built artifacts that you can download with [*actions/download-artifact*](https://github.com/actions/download-artifact).
>
> Unfortunately, v4 of both [is incompatible](https://github.blog/changelog/2023-12-14-github-actions-artifacts-v4-is-now-generally-available/) with previous versions, so you have to make sure that your *download-artifact* version matches the version that *build-and-inspect-python-package* uses for uploading.
>
> - If you’re using `download-artifact@v3`, you have to use `build-and-inspect-python-package@v1`.
> - If you’re using `download-artifact@v4`, you have to use `build-and-inspect-python-package@v2`.

While *build-and-inspect-python-package* will build a wheel for you by default, we recommend using [*cibuildwheel*] if your package contains compiled extensions.


### Inputs

- `path`: the location of the Python package to build (*optional*, default: `.`).
- `skip-wheel`: Whether to skip building the wheel in addition to the source distribution.
  The only meaningful value is `'true'` (note the quotes – GitHub Actions only allow string inputs) and everything else is treated as falsey.

  This is useful if you build your wheels using advanced tools like [*cibuildwheel*] anyway.
  (*optional*, default: `'false'`).
- `upload-name-suffix`: A suffix to append to the artifact names to make them unique for `upload-artifact@v4`.

  Use this if you want to build multiple packages in one workflow.
  (*optional*, default: `''`).
- `attest-build-provenance-github`: Whether to generate signed build provenance attestations for workflow artifacts using [actions/attest-build-provenance](https://github.com/actions/attest-build-provenance).
  Requires `attestations: write` and `id-token: write` permissions.
  The only meaningful value is `'true'` (note the quotes – GitHub Actions only allow string inputs) and everything else is treated as falsey.
  (*optional*, default: `'false'`).

> [!IMPORTANT]
> [GitHub's artifact attestations](https://docs.github.com/en/actions/security-for-github-actions/using-artifact-attestations/using-artifact-attestations-to-establish-provenance-for-builds) are different from PyPI's [Sigstore](https://www.sigstore.dev) attestations that you can generate while uploading using [*pypa/gh-action-pypi-publish*](https://github.com/pypa/gh-action-pypi-publish?tab=readme-ov-file#generating-and-uploading-attestations).


### Outputs

- `artifact-name`: The name of the uploaded artifact.

- `dist`: The location with the built packages.

  See, for example, how [*argon2-cffi-bindings*](https://github.com/hynek/argon2-cffi-bindings/blob/daff9ceb693312ab8257c60db4cd1c13cd866a35/.github/workflows/ci.yml#L83-L97) uses this feature to check the built wheels don’t break a package that depends on it.

- `supported_python_classifiers_json_array`: A JSON array of Python versions that are supported by the package as defined by the trove classifiers in the package metadata (for example, `Programming Language :: Python :: 3.12`).

  You can assign this to a matrix strategy key in your CI job (for example, `strategy.matrix.python-version`) to test against multiple Python versions without duplicating the information.
  Since GitHub Actions only allows for strings as variables, you have to parse it with `fromJSON` in your workflow.

  If all this sounds confusing: Check out our [supported Pythons CI workflow] for a realistic example.

- `supported_python_classifiers_json_job_matrix_value`: Same as `supported_python_classifiers_json_array`, but it’s a mapping with the JSON array bound to the `python-version` key.

  This is useful if you only want to define a matrix based on Python versions, because then you can just assign this to `strategy.matrix`.

- `package_name`: The name of the package as extracted from the package metadata.

- `package_version`: The version of the package as extracted from the package metadata.

  This is useful, for example, for displaying the PyPI URL on the GitHub UI for the publishing job:

  ```yaml
  jobs:
    ...
    release:
      runs-on: ubuntu-latest
      needs: baipp
      environment:
        name: pypi
        url: https://pypi.org/project/${{ needs.baipp.outputs.package-name }}/${{ needs.baipp.outputs.package-version }}
  ```

### Artifacts

After a successful run, you’ll find the following artifacts in the run’s Summary view:

- **Packages**: The built packages.
  Perfect for [automated PyPI upload workflows][automated]!
- **Package Metadata**: the extracted packaging metadata (*hint*: it’s formatted as an email).
- **PyPI README**: the extracted PyPI README, exactly how it would be used by PyPI as your project’s landing page.
  [PEP 621](https://peps.python.org/pep-0621/) calls it `readme`, in classic *setuptools* it’s `long_description`.


### Job Summaries

To save you from downloading the artifacts just to check their contents, *build-and-inspect-python-package* creates the following job summaries:

- **SDist contents**: A tree of the source distribution.
- **Wheel contents**: A tree of the built wheel – if one was built.
  This output has no timestamps because `wheel unpack` does not preserve them from the built wheel, leading to confusion.
- **Metadata**: A plain-text dump of package metadata (includes the PyPI README).


### Examples

[Our CI](.github/workflows/ci.yml) uses all inputs and outputs, if you want to see them in action.

Our [supported Pythons CI workflow] demonstrates how to use `supported_python_classifiers_json_array` to set up a matrix of Python versions for your CI jobs without duplicating the information with your packaging metadata.


## License

The scripts and documentation in this project are released under the [MIT License](LICENSE).

[automated]: https://github.com/python-attrs/attrs/blob/main/.github/workflows/pypi-package.yml
[*cibuildwheel*]: https://cibuildwheel.pypa.io/
[*setuptools-scm*]: https://setuptools-scm.readthedocs.io/
[supported Pythons CI workflow]: .github/workflows/ci-supported-pythons.yml

# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).


## [Unreleased](https://github.com/hynek/build-and-inspect-python-package/compare/v2.14.0...main)

### Changed

- Pick Python version explictly instead of `"3.x"` to prevent incompatibily problems like [#182](https://github.com/hynek/build-and-inspect-python-package/pull/182) in the future.


## [2.14.0](https://github.com/hynek/build-and-inspect-python-package/compare/v2.13.0...v2.14.0)

### Changed

- Update tools such that they work on Python 3.14 (which is now `3.x` on GitHub Actions).
  [#182](https://github.com/hynek/build-and-inspect-python-package/pull/182)

- The action now ignores `UV_PYTHON` coming from the outside.
  [#184](https://github.com/hynek/build-and-inspect-python-package/pull/184)


## [2.13.0](https://github.com/hynek/build-and-inspect-python-package/compare/v2.12.0...v2.13.0)

### Added

- New output: `package_name` is the name of the built package as stored in metadata.
  [#162](https://github.com/hynek/build-and-inspect-python-package/pull/162)

- The package name is now part of the action summary which is helpful when you build more than one package from a repository.
  [#169](https://github.com/hynek/build-and-inspect-python-package/pull/169)


### Changed

- All GitHub actions are now pinned to exact hashes for better reproducibility and mild security improvements[^st].

[^st]: Chosen prefix SHA-1 hash collision attacks [exist](https://eprint.iacr.org/2020/014.pdf). Against serious attackers, this is but security theater.


## [2.12.0](https://github.com/hynek/build-and-inspect-python-package/compare/v2.11.0...v2.12.0)

### Changed

- This release only updates the tools we use.
  It's important for being able to handle packaging metadata 2.4, as published by recent versions of Hatchling, though.
  [#161](https://github.com/hynek/build-and-inspect-python-package/pull/161)


## [2.11.0](https://github.com/hynek/build-and-inspect-python-package/compare/v2.10.0...v2.11.0)

### Added

- New output: `package_version` is the version of the package that was built.
  [#152](https://github.com/hynek/build-and-inspect-python-package/pull/152)


## [2.10.0](https://github.com/hynek/build-and-inspect-python-package/compare/v2.9.0...2.10.0)

### Changed

- Remove `.gitignore` from the build target directory to avoid [silly attestations](https://github.com/hynek/svcs/attestations/2821122).
  [#149](https://github.com/hynek/build-and-inspect-python-package/pull/149)


## [2.9.0](https://github.com/hynek/build-and-inspect-python-package/compare/v2.8.0...v2.9.0)

### Changed

- `uv build` is now used instead of the [*build*](https://pypi.org/project/build/) package.
  Since the actual build of the packages is done by the packaging backend (the one you define in your `pyproject.toml` under `build-system.build-backend` -- for example, Setuptools or Hatchling), this should make no difference except for faster runs.
  [#140](https://github.com/hynek/build-and-inspect-python-package/pull/140)


## [2.8.0](https://github.com/hynek/build-and-inspect-python-package/compare/v2.7.0...v2.8.0) - 2024-07-25

### Changed

- Use *uv*'s new `uv cache prune --ci` to only cache downloaded files.
  This makes the cache smaller and faster to pack/unpack.
  [#135](https://github.com/hynek/build-and-inspect-python-package/pull/135)


### Fixed

- Turns out, the default location of *uv*'s cache cannot be cached and [*actions/cache*](https://github.com/actions/cache) fails silently with an opaque "Path(s) specified in the action for caching do(es) not exist, hence no cache is being saved." log message.
  We have moved the cache to `/tmp`.
  [#135](https://github.com/hynek/build-and-inspect-python-package/pull/135)


## [2.7.0](https://github.com/hynek/build-and-inspect-python-package/compare/v2.6.0...v2.7.0) - 2024-07-17

### Added

- A header before package contents in the summary.
  Especially useful together with a preceding build provenance attestation.
  [#131](https://github.com/hynek/build-and-inspect-python-package/pull/131)


## [2.6.0](https://github.com/hynek/build-and-inspect-python-package/compare/v2.5.0...v2.6.0) - 2024-05-26

### Added

- Support for `ubuntu-24.04` builders.
  [#126](https://github.com/hynek/build-and-inspect-python-package/pull/126)

- New output: `artifact-name` is the name of the uploaded artifact.
  [#125](https://github.com/hynek/build-and-inspect-python-package/pull/125)


## [2.5.0](https://github.com/hynek/build-and-inspect-python-package/compare/v2.4.0...v2.5.0) - 2024-05-13

### Added

- New input: `attest-build-provenance-github` generates signed build provenance attestations for workflow artifacts.
  [#122](https://github.com/hynek/build-and-inspect-python-package/pull/122)


## [2.4.0](https://github.com/hynek/build-and-inspect-python-package/compare/v2.3.0...v2.4.0) - 2024-04-11

### Changed

- The action doesn't crash anymore if the user sets globally the `UV_SYSTEM_PYTHON` environment variable.
  [#116](https://github.com/hynek/build-and-inspect-python-package/pull/116)


## [2.3.0](https://github.com/hynek/build-and-inspect-python-package/compare/v2.2.1...v2.3.0) - 2024-04-11

### Added

- Cache busting for the *uv* cache.
  GitHub Actions's caching behavior is a bit idiosyncratic:
  Once a cache is created, it's immutable.
  But as long as it's accessed within 7 days, it never goes away.

  Therefore, *baipp* now uses the hash of the requirements file as part of the cache key.
  Behaviorally, nothing changes, except that the cache doesn't grow useless over time.
  [#115](https://github.com/hynek/build-and-inspect-python-package/pull/115)


## [2.2.1](https://github.com/hynek/build-and-inspect-python-package/compare/v2.2.0...v2.2.1) - 2024-04-02

### Fixed

- The action uses *wheel* to unpack wheels again (this is a revert of [#103](https://github.com/hynek/build-and-inspect-python-package/pull/103)) due to [incompatibilities](https://github.com/hynek/build-and-inspect-python-package/issues/113) with, for example, *pytest*.
  To avoid the confusion due to wrong timestamps, the wheel's tree output in the Summary has no timestamps anymore.
  [#114](https://github.com/hynek/build-and-inspect-python-package/pull/114)


## [2.2.0](https://github.com/hynek/build-and-inspect-python-package/compare/v2.1.0...v2.2.0) - 2024-03-31

### Changed

- Use *uv* as installer command for *build* for further speedups.
  [#107](https://github.com/hynek/build-and-inspect-python-package/pull/107)


## [2.1.0](https://github.com/hynek/build-and-inspect-python-package/compare/v2.0.2...2.1.0) - 2024-03-27

### Added

- New outputs: `supported_python_classifiers_json_array` and `supported_python_classifiers_json_job_matrix_value`.

  They are extracted from the trove classifiers defined in the package metadata (for example, `Programming Language :: Python :: 3.12`) and allow you to define the Python versions matrix for your CI jobs without duplicating this information.
  [#80](https://github.com/hynek/build-and-inspect-python-package/pull/80)
  [#102](https://github.com/hynek/build-and-inspect-python-package/pull/102)

- New input: `skip-wheel` to skip building the wheel in addition to the source distribution.
  This is useful if you need to build your wheels using advanced tools like [*cibuildwheel*](https://cibuildwheel.pypa.io/) anyway.
  [#98](https://github.com/hynek/build-and-inspect-python-package/pull/98)

- New input: `upload-name-suffix` allows to build more than one package in a single workflow by distinguishing the artifact names.
  [#97](https://github.com/hynek/build-and-inspect-python-package/pull/97)


### Changed

- The action now uses [*uv*](https://github.com/astral-sh/uv) to install its tools to speed up your CI runs.
  [#86](https://github.com/hynek/build-and-inspect-python-package/pull/86)

- We now use `unzip` to extract wheels, which preserves timestamps in the "Wheel contents" summary.
  [#103](https://github.com/hynek/build-and-inspect-python-package/pull/103)


## [2.0.2](https://github.com/hynek/build-and-inspect-python-package/compare/v2.0.1...v2.0.2) – 2024-03-16

### Changed

- Dependency updates for Metadata-Version 2.3 support (as used, for example, by [Hatchling 1.22.1](https://github.com/pypa/hatch/releases/tag/hatchling-v1.22.1)).


## [2.0.1](https://github.com/hynek/build-and-inspect-python-package/compare/v2.0.0...v2.0.1) - 2024-01-25

### Changed

- Switched to `setup-python@v5` to avoid the "Node.js 16 actions are deprecated." deprecation warning.


## [2.0.0](https://github.com/hynek/build-and-inspect-python-package/compare/v1.5.4...v2.0.0) - 2023-12-15

### Changed

- Switched to using v4 of `actions/upload-artifact`.
  This version is incompatible with older versions of `actions/download-artifact` -- hence the major version bump.
  See also [GitHub's announcement](https://github.blog/changelog/2023-12-14-github-actions-artifacts-v4-is-now-generally-available/).
  [#78](https://github.com/hynek/build-and-inspect-python-package/pull/78)


## [1.5.4](https://github.com/hynek/build-and-inspect-python-package/compare/v1.5.3...v1.5.4) - 2023-11-01

### Fixed

- Stop trying to cache.
  Fixes `Error: No file in /home/runner/work/pytest-cpp/pytest-cpp matched to [**/requirements.txt or **/pyproject.toml], make sure you have checked out the target repository`
  [#76](https://github.com/hynek/build-and-inspect-python-package/pull/76)


## [1.5.3](https://github.com/hynek/build-and-inspect-python-package/compare/v1.5.1...1.5.3) - 2023-10-27

### Changed

- Hopefully nothing, but this release comes from the main branch again.


## [1.5.2](https://github.com/hynek/build-and-inspect-python-package/compare/v1.5...v1.5.2) - 2023-10-27

### Fixed

- Turns out it made a huge difference.
  This release is branched directly from v1.5 and only updates the dependencies.


## [1.5.1](https://github.com/hynek/build-and-inspect-python-package/compare/v1.5...v1.5.1) - 2023-10-27

### Changed

- Updates of the tools we use.
  Notably this fixes *check-wheel-contents* on Python 3.12.

- This shouldn't make any difference, but all management and command running is now done by [PDM](https://pdm.fming.dev/).
  [#57](https://github.com/hynek/build-and-inspect-python-package/pull/57)


## [1.5.0](https://github.com/hynek/build-and-inspect-python-package/compare/v1.4.1...v1.5.0) - 2023-02-09

### Added

- Set [`SOURCE_DATE_EPOCH`](https://reproducible-builds.org/specs/source-date-epoch/) based on the timestamp of the last commit for build reproducibility.
  [#30](https://github.com/hynek/build-and-inspect-python-package/pull/30)
- The *tree* output now has ISO timestamps.


### Changed

- Use `3.x` version specifier in `setup-python` for the venv used by our tools.
  As of writing, that's 3.11.


## [1.4.1](https://github.com/hynek/build-and-inspect-python-package/compare/v1.4...v1.4.1) - 2022-10-13

### Fixed

- Doesn't raise a [deprecation warning](https://github.blog/changelog/2022-10-11-github-actions-deprecating-save-state-and-set-output-commands/) re: `set-output` anymore.


## [1.4](https://github.com/hynek/build-and-inspect-python-package/compare/v1.3...v1.4) - 2022-10-13

### Added

- The contents listing of the SDist, the contents listing of the wheel, *and* the package metadata are now conveniently added to the CI run summary.
  So, you don't have to click through the build logs or download anything to give it a quick glimpse.
  [#17](https://github.com/hynek/build-and-inspect-python-package/pull/17)


### Fixed

- The Python environment that is used by us is better isolated.
  [#18](https://github.com/hynek/build-and-inspect-python-package/pull/18)


## [1.3](https://github.com/hynek/build-and-inspect-python-package/compare/v1.2...v1.3) - 2022-08-24

### Added

- Package metadata is now printed and uploaded as an artifact.
  [#11](https://github.com/hynek/build-and-inspect-python-package/pull/11)
- The PyPI README, that is a project's PyPI landing page, is now uploaded as an artifact.
  [#11](https://github.com/hynek/build-and-inspect-python-package/pull/11)


## [1.2](https://github.com/hynek/build-and-inspect-python-package/compare/v1.1...v1.2) - 2022-08-21

### Added

- The location of the built packages is now returned using the `dist` output.
  [#9](https://github.com/hynek/build-and-inspect-python-package/pull/9)


## [1.1](https://github.com/hynek/build-and-inspect-python-package/compare/v1.0...v1.1) - 2022-08-20

### Changed

- Built packages are written to `/tmp` instead of into the source `dist/` directory. That means that this action leaves your directory clean.
  [#7](https://github.com/hynek/build-and-inspect-python-package/pull/7)


## [1.0](https://github.com/hynek/build-and-inspect-python-package/compare/v0.1...v1.0) - 2022-08-20

### Changed

- *twine* now runs in `--strict` mode.
- All tools are now pinned reliability (and hopefully caching in the future).
- The action now uses `setup-python` itself to enable pinning (and hopefully caching in the future). Currently, Python 3.10 is used.
- The tools are installed into a virtual environment. That means you can re-use the global Python environment and do further checks.


## [0.1](https://github.com/hynek/build-and-inspect-python-package/tree/v0.1) - 2022-08-20

### Added

- Initial Release

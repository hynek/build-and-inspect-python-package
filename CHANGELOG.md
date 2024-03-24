# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).


## [UNRELEASED](https://github.com/hynek/build-and-inspect-python-package/compare/v2.0.2...main)

### Added

- New input: `skip-wheel` to skip building the wheel in addition to the source distribution.
  This is useful if you need to build your wheels using advanced tools like [*cibuildwheel*](https://cibuildwheel.pypa.io/) anyway.
  [#98](https://github.com/hynek/build-and-inspect-python-package/pull/98)


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

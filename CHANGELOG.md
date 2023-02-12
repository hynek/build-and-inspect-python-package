# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).


## [Unreleased](https://github.com/hynek/build-and-inspect-python-package/compare/v1.5...main)


## [1.5](https://github.com/hynek/build-and-inspect-python-package/compare/v1.4.1...v1.5)

### Added

- Set [`SOURCE_DATE_EPOCH`](https://reproducible-builds.org/specs/source-date-epoch/) based on the timestamp of the last commit for build reproducibility.
  [#30](https://github.com/hynek/build-and-inspect-python-package/pull/30)
- The *tree* output now has ISO timestamps.


### Changed

- Use `3.x` version specifier in `setup-python` for the venv used by our tools.
  As of writing, that's 3.11.


## [1.4.1](https://github.com/hynek/build-and-inspect-python-package/compare/v1.4...v1.4.1)

### Fixed

- Doesn't raise a [deprecation warning](https://github.blog/changelog/2022-10-11-github-actions-deprecating-save-state-and-set-output-commands/) re: `set-output` anymore.


## [1.4](https://github.com/hynek/build-and-inspect-python-package/compare/v1.3...v1.4)

### Added

- The contents listing of the SDist, the contents listing of the wheel, *and* the package metadata are now conveniently added to the CI run summary.
  So, you don't have to click through the build logs or download anything to give it a quick glimpse.
  [#17](https://github.com/hynek/build-and-inspect-python-package/pull/17)


### Fixed

- The Python environment that is used by us is better isolated.
  [#18](https://github.com/hynek/build-and-inspect-python-package/pull/18)


## [1.3](https://github.com/hynek/build-and-inspect-python-package/compare/v1.2...v1.3)

### Added

- Package metadata is now printed and uploaded as an artifact.
  [#11](https://github.com/hynek/build-and-inspect-python-package/pull/11)
- The PyPI README, that is a project's PyPI landing page, is now uploaded as an artifact.
  [#11](https://github.com/hynek/build-and-inspect-python-package/pull/11)


## [1.2](https://github.com/hynek/build-and-inspect-python-package/compare/v1.1...v1.2)

### Added

- The location of the built packages is now returned using the `dist` output.
  [#9](https://github.com/hynek/build-and-inspect-python-package/pull/9)


## [1.1](https://github.com/hynek/build-and-inspect-python-package/compare/v1.0...v1.1)

### Changed

- Built packages are written to `/tmp` instead of into the source `dist/` directory. That means that this action leaves your directory clean.
  [#7](https://github.com/hynek/build-and-inspect-python-package/pull/7)


## [1.0](https://github.com/hynek/build-and-inspect-python-package/compare/v0.1...v1.0)

### Changed

- *twine* now runs in `--strict` mode.
- All tools are now pinned reliability (and hopefully caching in the future).
- The action now uses `setup-python` itself to enable pinning (and hopefully caching in the future). Currently, Python 3.10 is used.
- The tools are installed into a virtual environment. That means you can re-use the global Python environment and do further checks.


## [0.1](https://github.com/hynek/build-and-inspect-python-package/tree/v0.1) - 2022-08-20

### Added

- Initial Release

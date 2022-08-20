# Changelog
All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).


## [Unreleased](https://github.com/hynek/build-and-inspect-python-package/compare/v1.1...main)


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

  See for example how [*argon2-cffi-bindings*](https://github.com/hynek/argon2-cffi-bindings/blob/7427184b2beb5ec5e7f8c8a9a6e3c6409c81aeae/.github/workflows/ci.yml#L66-L79) uses it to check the wheels don't break a dependency.


## [0.1](https://github.com/hynek/build-and-inspect-python-package/tree/v0.1) - 2022-08-20

### Added

- Initial Release

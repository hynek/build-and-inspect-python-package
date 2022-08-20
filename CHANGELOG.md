# Changelog
All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).


## [Unreleased](https://github.com/hynek/build-and-inspect-python-package/compare/v0.1...main)

### Changed

- *twine* now runs in `--strict` mode.
- All tools are now pinned and cached to improve performance and reliability.
- The action now uses `setup-python` itself to enable pinning and caching.
  Currently, Python 3.10 is used.
- The tools are installed into a virtual environment.
  That means you can re-use the global Python environment and do further checks.

  See for example how [*argon2-cffi-bindings*](https://github.com/hynek/argon2-cffi-bindings/blob/1bb072cdba857bc22c3fa1d976659279d1c08a23/.github/workflows/main.yml#L70-L79) uses it to check the wheels don't break a dependency.


## [0.1](https://github.com/hynek/build-and-inspect-python-package/tree/v0.1) - 2022-08-20

### Added

- Initial Release

# demo-python-package

[![Travis CI](https://img.shields.io/travis/astropenguin/demo-python-package/master.svg?label=Travis%20CI&style=flat-square)](https://travis-ci.org/astropenguin/demo-python-package)
[![Codecov](https://img.shields.io/codecov/c/github/astropenguin/demo-python-package?label=Codecov&style=flat-square)](https://codecov.io/gh/astropenguin/demo-python-package)
[![linting](https://img.shields.io/badge/Linting-Flake8-orange?style=flat-square)](http://flake8.pycqa.org/en/latest)
[![Formatting](https://img.shields.io/badge/Formatting-Black-333?style=flat-square)](https://black.readthedocs.io/en/stable)
[![Testing](https://img.shields.io/badge/Testing-pytest-yellow?style=flat-square)](https://docs.pytest.org/en/latest)
[![License](https://img.shields.io/badge/license-MIT-blue.svg?label=License&style=flat-square)](LICENSE)

:gift: A repository to demonstrate the development of a Python package

## Overview

This repository demonstrates a way to develop a well-formatted and well-tested Python package using CI/CD.
With a simple `demo` Python package, the following essential (but laborious) processes are automatically run by [Travis CI] in the cloud, which enables developers to keep focusing on the development of the package itself.

- code linting (by [Flake8])
- code formatting (by [Black])
- code testing (by [pytest])
- code coverage (by [Codecov])
- documentation building (by [Sphinx])
- documentation hosting (by [GitHub Pages])

## Local Python environment

This repository has [Pipenv] files (`Pipfile` and `Pipfile.lock`) which describe a specific Python version and dependent packages.
After cloning the repository to local, you can initialize a Python environment as follows:

```bash
$ git clone https://github.com/astropenguin/demo-python-package.git
$ cd demo-python-package
$ pipenv install --dev
```

Note that the following examples are assumed to be run in a virtual environment created by Pipenv:

```bash
$ pipenv shell # enter a virtual environment
(demo-python-package) $
```

## Code linting by Flake8

[Flake8] is a linter which checks that the source code follows the Python style guide.
The following command checks all the code in the `demo` package:

```bash
$ flake8 demo
$ # nothing is output if no errors
```

## Code formatting by Black

[Black] is a formatter which (forcibly) reformats the source code according to [the Black code style](https://black.readthedocs.io/en/stable/the_black_code_style.html).
The following command reformats all the code in the `demo` package **in place**:

```bash
$ black demo
All done! ✨ 🍰 ✨
1 file would be left unchanged.
$
```

If you want to just check them, run with `--check` option:

```bash
$ black --check demo
All done! ✨ 🍰 ✨
1 file would be left unchanged.
$
```

## Code testing by pytest

[pytest] runs all test scripts in the `tests` directory.
Before using it, you need to install the `demo` package by pip so that the scripts can find the path of the package:

```bash
$ pip install -e .
$ pytest
========================= test session starts =========================
platform darwin -- Python 3.7.3, pytest-5.1.1, py-1.8.0, pluggy-0.12.0
rootdir: /path/to/demo-python-package
plugins: cov-2.7.1
collected 2 items

tests/test_version.py ..                                        [100%]

========================== 2 passed in 0.02s ==========================
$
```

## Code coverage by Codecov

[Codecov] calculates the fraction of the code that are tested to the whole code (often called code coverage), which is a useful indicator that the package is well-tested or not.
The code coverage is calculated by pytest with `--cov` option:

```bash
$ pytest --cov demo
========================= test session starts =========================
platform darwin -- Python 3.7.3, pytest-5.1.1, py-1.8.0, pluggy-0.12.0
rootdir: /path/to/demo-python-package
plugins: cov-2.7.1
collected 2 items

tests/test_version.py ..                                        [100%]

---------- coverage: platform darwin, python 3.7.3-final-0 -----------
Name               Stmts   Miss  Cover
--------------------------------------
demo/__init__.py       3      0   100%

========================== 2 passed in 0.05s ==========================
$
```

## Documentation building by Sphinx

[Sphinx] is a document builder written in Python.
In this repository, it generates an HTML document that describes the usage of the package functions, classes, etc based on the docstrings of them.
In the local environment, you can generate the document by the following commands:

```bash
$ sphinx-apidoc -f -o docs/_apidoc demo
$ sphinx-build docs docs/_build
$ sphinx-build docs docs/_build
```

## Documentation hosting by GitHub Pages

Once the document is created, it is hosted by [GitHub Pages].
The document is then pushed to the `gh-pages` branch by [Travis CI].
See `.travis.yml` for more details.

## Useful tips

### VS Code settings

If you use VS Code, the following settings (to be written in `.vscode/settings.json`) enables the editor integration of Black and Flake8.
This would be useful to check linting and do formatting of the package on the spot.
Note that you need to install [the Python extension](https://marketplace.visualstudio.com/items?itemName=ms-python.python) to VS Code.

```jsonc
{
    // auto formatting by Black
    "editor.formatOnType": true,
    "python.formatting.provider": "black",
    // auto linting by Flake8
    "python.linting.enabled": true,
    "python.linting.lintOnSave": true,
    "python.linting.flake8Enabled": true,
    "python.linting.pylintEnabled": false,
}
```

### Linting and formatting before commit

If you want to make sure that the code follows Flake8 and Black before you commit, [pre-commit] and the following settings (to be written in `.pre-commit-config.yaml`) is useful.
This will check linting and formatting of the code you stage before you commit and the commit will be passed only when Black and Flake8 finish with the status code 0.

```yaml
repos:
  - repo: https://github.com/ambv/black
    rev: stable
    hooks:
      - id: black
        language_version: python3.7
  - repo: https://gitlab.com/pycqa/flake8
    rev: 3.7.8
    hooks:
      - id: flake8
```

Note that you need to run the following command before you use the feature.

```bash
$ pre-commit install
```

## References

- [Travis CI]
    - [Build Stages \- Travis CI](https://docs.travis-ci.com/user/build-stages)
    - [Job Lifecycle \- Travis CI](https://docs.travis-ci.com/user/job-lifecycle)
- [Flake8]
    - [Using Version Control Hooks — flake8 3\.7\.8 documentation](http://flake8.pycqa.org/en/latest/user/using-hooks.html)
    - [black/\.flake8 at master · psf/black](https://github.com/psf/black/blob/master/.flake8)
- [Black]
    - [Version control integration — Black 19\.3b0 documentation](https://black.readthedocs.io/en/stable/version_control_integration.html)
    - [Editor integration — Black 19\.3b0 documentation](https://black.readthedocs.io/en/stable/editor_integration.html)
- [Sphinx]
    - [Getting Started — Sphinx 2\.3\.0 documentation](https://www.sphinx-doc.org/en/2.0/usage/quickstart.html)
    - [Support for NumPy and Google style docstrings — Sphinx 2\.3\.0 documentation](https://www.sphinx-doc.org/en/2.0/usage/extensions/napoleon.html)
- [Codecov]
    - [codecov/example\-python: Python coverage example](https://github.com/codecov/example-python)
- [GitHub Pages]
    - [GitHub Pages Deployment \- Travis CI](https://docs.travis-ci.com/user/deployment/pages/)
- [pytest]
    - [Installing and Using plugins — pytest documentation](https://docs.pytest.org/en/latest/plugins.html)
- [pre-commit]
    - [pre\-commit](https://pre-commit.com/hooks.html)
- [Pipenv]

[Travis CI]: https://travis-ci.org
[Flake8]: http://flake8.pycqa.org/en/latest
[Black]: https://black.readthedocs.io/en/stable
[pytest]: https://docs.pytest.org/en/latest
[Codecov]: https://codecov.io
[Sphinx]: http://www.sphinx-doc.org/en/master
[GitHub Pages]: https://pages.github.com
[Pipenv]: https://pipenv.readthedocs.io/en/latest
[pre-commit]: https://pre-commit.com/

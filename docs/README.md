## Get Started

![unimport](https://raw.githubusercontent.com/hakancelik96/unimport/master/images/logo/Unimport.png)

**A linter & formatter for finding & removing unused import statements.**

![Lint](https://github.com/hakancelik96/unimport/workflows/Lint/badge.svg)
![Test](https://github.com/hakancelik96/unimport/workflows/Test/badge.svg)

[![Forks](https://img.shields.io/github/forks/hakancelik96/unimport)](https://github.com/hakancelik96/unimport/fork)
[![Issues](https://img.shields.io/github/issues/hakancelik96/unimport)](https://github.com/hakancelik96/unimport/issues)
[![License](https://img.shields.io/github/license/hakancelik96/unimport.svg)](https://github.com/hakancelik96/unimport/blob/master/LICENSE)
![PyPI - Python Version](https://img.shields.io/pypi/pyversions/unimport)
[![Pypi](https://img.shields.io/pypi/v/unimport)](https://pypi.org/project/unimport/)
[![Stars](https://img.shields.io/github/stars/hakancelik96/unimport)](https://github.com/hakancelik96/unimport/stargazers)

[![Codacy Badge](https://api.codacy.com/project/badge/Grade/3fbd4686e97b4e19906ca2fa933e4cfc)](https://app.codacy.com/manual/hakancelik96/unimport?utm_source=github.com&utm_medium=referral&utm_content=hakancelik96/unimport&utm_campaign=Badge_Grade_Settings)
[![Codecov](https://codecov.io/gh/hakancelik96/unimport/branch/master/graph/badge.svg)](https://codecov.io/gh/hakancelik96/unimport)
[![Style](https://img.shields.io/badge/style-black-black)](https://github.com/psf/black)

[![Contributors](https://img.shields.io/github/contributors/hakancelik96/unimport)](https://github.com/hakancelik96/unimport/graphs/contributors)
[![Last Commit](https://img.shields.io/github/last-commit/hakancelik96/unimport.svg)](https://github.com/hakancelik96/unimport/commits/master)

[![Pepy](https://pepy.tech/badge/unimport)](https://pepy.tech/badge/unimport)

## Installation and Usage

### Installation

Unimport requires Python 3.6+ and can be easily installed using most common Python
packaging tools. We recommend installing the latest stable release from PyPI with pip:

```shell
$ pip install unimport
```

### Usage

```shell
$ unimport [sources [sources ...]]
```

_If you do not get any output, congratulations means there is no unused import in your
project._

#### Let's with example with simple a Python code

**example.py**

```python
import t
from l import t
from x import y, z, t

def function(f=t):
    import x
    return f

from i import t, ii

print(t)
```

## `--check` flag

> Prints which file the unused imports are in.

When the `--diff`, `--permission` and `--remove` flags are used, the `--check` flag set
as `False` If you still want to see the results, use the `--check` flag.

### Usage

- `$ unimport example.py`
- `$ unimport example.py --check`
- `$ unimport example.py --diff --check`
- `$ unimport example.py --check --diff --remove`

```shell
$ unimport example.py

t at example.py:1
t at example.py:2
y at example.py:3
z at example.py:3
x at example.py:6
ii at example.py:9
```

## `-d, --diff` flag

> Prints a diff of all the changes unimport would make to a file.

### Usage

- `$ unimport example.py -d`
- `$ unimport example.py --diff`

```python
$ unimport example.py -d
--- example.py

+++

@@ -1,11 +1,8 @@

-import t
-from l import t
-from x import y, z, t
+from x import t

 def function(f=t):
-    import x
     return f

-from i import t, ii
+from i import t

print(t)
```

## `-p, --permission` flag

> Refactor permission after see diff.

### Usage

- `$ unimport example.py -p`
- `$ unimport example.py --permission`

```python
$ unimport example.py -p
--- example.py

+++

@@ -1,11 +1,8 @@

-import t
-from l import t
-from x import y, z, t
+from x import t

 def function(f=t):
-    import x
     return f

-from i import t, ii
+from i import t

 print(t)
Apply suggested changes to 'example.py' [Y/n/q] ? >
```

## `-r, --remove` flag

> remove unused imports automatically.

#### Usage

- `$ unimport example.py -r`
- `$ unimport example.py --remove`

`$ unimport example.py -r`

```python
from x import t

def function(f=t):
    return f

from i import t

print(t)
```

## `--show-error` flag

> Show or don't show errors captured during static analysis.

Use this flag if you want to see errors ( like `SyntaxError` ) during analysis.

### Usage

- `$ unimport example.py --show-error`

## `--include-star-import` flag

> Include star imports during scanning and refactor.

**/example.py**

```python
from os import *

for i in walk("."):
  print(i)
```

```shell
$ unimport example.py --include-star-import

os at example.py:1 from os import walk
```

```shell
$ unimport example.py --include-star-import --diff
--- example.py

+++

@@ -1,4 +1,4 @@

-from os import *
+from os import walk

 for i in walk("."):
   print(i)
```

## Typing Comments

**This feature is only available for python 3.8.**

Imports in the example below aren't flag as unused by import.

```python
from typing import Any
from typing import Tuple
from typing import Union
def function(a, b):
    # type: (Any, str) -> Union[Tuple[None, None], Tuple[str, str]]
    pass
```

For more information

[PEP 526 - Syntax for Variable Annotations](https://www.python.org/dev/peps/pep-0526/)

## Skip Import

Leave '# unimport: skip' or '# noqa' at the end of the line to skip imports **for
example:**

```python
import x # unimport:skip
```

```python
from x import ( # noqa
  t, y,
  f, r
)
```

## `__all__`

Unimport looks at the items in the `__all__` list, if it matches the imports, marks it
as being used.

```python
import os

__all__ = ["os"] # this import is used and umimport can understand
```

## Command line options

You can list many options by running unimport --help

```bash
usage: unimport [-h] [-c PATH] [--include include] [--exclude exclude]
                [--include-star-import] [--show-error] [-d] [-r | -p]
                [--check] [-v]
                [sources [sources ...]]

A linter, formatter for finding and removing unused import statements.

positional arguments:
  sources               files and folders to find the unused imports.

optional arguments:
  -h, --help            show this help message and exit
  -c PATH, --config PATH
                        read configuration from PATH.
  --include include     file include pattern.
  --exclude exclude     file exclude pattern.
  --include-star-import
                        Include star imports during scanning and refactor.
  --show-error          Show or don't show errors captured during static
                        analysis.
  -d, --diff            Prints a diff of all the changes unimport would make
                        to a file.
  -r, --remove          remove unused imports automatically.
  -p, --permission      Refactor permission after see diff.
  --check               Prints which file the unused imports are in.
  -v, --version         Prints version of unimport

Get rid of all unused imports 🥳
```

## Configuring Unimport

It's possible to configure **unimport** from `pyproject.toml` or `setup.cfg` files if
you have.

If you want to use `pyproject.toml` to configure, you must to install it. Try this;
`pip install toml==0.10.0`

- `exclude` file exclude pattern.
- `include` file include pattern.

For example:

**pyproject.toml**

```ini
[tool.unimport]
exclude = '(__init__.py)|env'
include = 'my_project'
```

**setup.cfg**

```ini
[unimport]
exclude = (__init__.py)|env
include = my_project
```

## Adding pre-commit plugins to your project

Once you have [pre-commit](https://pre-commit.com/)
[installed](https://pre-commit.com/#install), adding pre-commit plugins to your project
is done with the .pre-commit-config.yaml configuration file.

Add a file called .pre-commit-config.yaml to the root of your project. The pre-commit
config file describes what repositories and hooks are installed.

```yaml
repos:
  - repo: https://github.com/hakancelik96/unimport
    rev: v0.2.8
    hooks:
      - id: unimport
        args: [-r, --include-star-import]
```

## Our badge

[![style](https://img.shields.io/badge/unimport-v0.2.8-red)](https://github.com/hakancelik96/unimport)

**Please insert this badge into your project**

`[![style](https://img.shields.io/badge/unimport-v0.2.8-red)](https://github.com/hakancelik96/unimport)`

## CONTRIBUTING

[![](https://sourcerer.io/fame/hakancelik96/hakancelik96/unimport/images/0)](https://sourcerer.io/fame/hakancelik96/hakancelik96/unimport/links/0)[![](https://sourcerer.io/fame/hakancelik96/hakancelik96/unimport/images/1)](https://sourcerer.io/fame/hakancelik96/hakancelik96/unimport/links/1)[![](https://sourcerer.io/fame/hakancelik96/hakancelik96/unimport/images/2)](https://sourcerer.io/fame/hakancelik96/hakancelik96/unimport/links/2)[![](https://sourcerer.io/fame/hakancelik96/hakancelik96/unimport/images/3)](https://sourcerer.io/fame/hakancelik96/hakancelik96/unimport/links/3)[![](https://sourcerer.io/fame/hakancelik96/hakancelik96/unimport/images/4)](https://sourcerer.io/fame/hakancelik96/hakancelik96/unimport/links/4)[![](https://sourcerer.io/fame/hakancelik96/hakancelik96/unimport/images/5)](https://sourcerer.io/fame/hakancelik96/hakancelik96/unimport/links/5)[![](https://sourcerer.io/fame/hakancelik96/hakancelik96/unimport/images/6)](https://sourcerer.io/fame/hakancelik96/hakancelik96/unimport/links/6)[![](https://sourcerer.io/fame/hakancelik96/hakancelik96/unimport/images/7)](https://sourcerer.io/fame/hakancelik96/hakancelik96/unimport/links/7)

[CONTRIBUTING.md](/CONTRIBUTING.md)

## CHANGELOG

All notable changes to this project will be documented in this file.

[CHANGELOG.md](/CHANGELOG.md)

## Contact

- [![Telegram](https://img.shields.io/badge/telegram-@hakancelik-brightgreen?logo=telegram)](https://t.me/hakancelik96)
- [![Twitter](https://img.shields.io/twitter/follow/hakancelik96?style=social)](https://twitter.com/hakancelik96)
- [![Github](https://img.shields.io/github/followers/hakancelik96?label=hakancelik96&style=social)](https://github.com/hakancelik96)
- <hakancelik96@outlook.com>

## Who's using Unimport

[![radity.com](https://raw.githubusercontent.com/hakancelik96/unimport/master/images/clients/radity.jpg)](https://radity.com/?ref=unimport)
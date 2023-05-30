<!--
This file is part of OpenScan-Hardware
Copyright 2023 OpenScan Contributors
SPDX-License-Identifier: CC-BY-SA-4.0
-->

# OpenScan Hardware

This repository contains the source files for the OpenScan Hardware book, a
**work in progress**. Further details will be added.

## Building

Clone this repository and run the following commands (Linux/macOS):

```sh
python -m venv venv
echo '*' >venv/.gitignore  # Do not track virtual environment
source venv/bin/activate
pip install -r book/requirements.txt
jupyter-book build book
```

On Windows:

```cmd
python -m venv venv
echo "*" >venv\.gitignore
venv\Scripts\activate
pip install -r book\requirements.txt
jupyter-book build book
```

The HTML site is generated at `book/_build/html/index.html`.

We use a [pre-commit](https://pre-commit.com/) hook to automatically normalize
formatting and perform a few checks when creating Git commits. To set this up:

```sh
pre-commit install
```

(If you don't have the `pre-commit` command globally installed, you can install
it in the virtual environment using `pip install pre-commit` and it should work
as long as the virtual environment is activated when you commit.)

# Linting
Run pylint over your code using [this pylintrc](.pylintrc).

Advantages and disadvantages of linting can be found in the [Google python styling guide](https://google.github.io/styleguide/pyguide.html).

> pylint is a tool for finding bugs and style problems in Python source code. It finds problems that are typically caught by a compiler for less dynamic languages like C and C++. Because of the dynamic nature of Python, some warnings may be incorrect; however, spurious warnings should be fairly infrequent.

Suppress warnings if they are inappropriate so that other issues are not hidden. To suppress warnings, you can set a line-level comment:

```python
dict = 'something awful'  # Bad Idea... pylint: disable=redefined-builtin
```

`pylint` warnings are each identified by symbolic name (`empty-docstring`).

Suppressing in this way has the advantage that we can easily search for suppressions and revisit them.

You can get a list of pylint warnings by doing:
```sh
pylint --list-msgs
```
To get more information on a particular message, use:
```sh
pylint --help-msg=C6409
```

To execute `pylint` around the entire project a search for all `.py` files can be used like the following:
```sh
find . -name "*.py" | xargs pylint
```

To execute `pylint` around the entire project, and also print out the file path in any errors in a VS code format you can run the following:
```sh
find . -name "*.py" | xargs pylint --msg-template="{path}({line}): [{obj}] {msg} ({symbol})"
```

The following github action can be used to automate linting on any of your repository pull requests
```yaml
name: Pylint

on: [push]

jobs:
  pylint:
    name: Lint Code
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v2
    - name: Set up Python 2.7
      uses: actions/setup-python@v1
      with:
        python-version: 2.7
    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip
        pip install pylint
    - name: Run pylint and ensure a perfect score
      run: find . -name '*.py' | xargs pylint;
```

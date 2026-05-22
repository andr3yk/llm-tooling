---
name: python-style
description: >-
  Applies Google Python Style Guide–aligned conventions and modern Python hygiene: imports, exceptions, typing, pathlib and resources, docstrings, and Ruff/mypy or pyright checks. Use when writing or reviewing Python, fixing sloppy patterns, adding types, refactoring Python modules, or when the user mentions pyguide, PEP 8, ruff, mypy, or Python style. Do not use for non-Python projects or high-level architectural design without python-specific concerns.
---

# Python style and quality

Execute the following steps chronologically when writing, reviewing, or refactoring Python code.

## Step 1: Enforce tooling and configuration

1. Run `ruff check` and `ruff format` if the project uses Ruff.
2. Run `mypy` or `pyright` per project configuration.
3. Fix types instead of using blanket `# type: ignore`. Use targeted ignores with codes when required.

## Step 2: Format imports

1. Remove any `from module import *` (except rare re-exports with `__all__`).
2. Order imports: future -> stdlib -> third party -> local, with a blank line between groups.
3. Use absolute imports for package code. Use relative imports only inside explicit packages.
4. Use `TYPE_CHECKING` guards for conditional imports to avoid circular dependencies.

## Step 3: Handle exceptions properly

1. Use `raise SpecificError("message")` with a specific exception type.
2. Remove bare `except:` and bare `except BaseException:` unless re-raising or shutting down.
3. Use exception chaining when wrapping: `raise NewError(...) from err`.

## Step 4: Fix mutable defaults and late binding

1. Replace mutable defaults (`def f(x=[])` or `def f(d={})`) with `None` and assign inside the function, or use `dataclasses.field(default_factory=...)`.
2. Fix closures in loops by using `lambda x=x:` or `functools.partial` so each closure captures the intended value.

## Step 5: Apply typing

1. Annotate public functions and methods.
2. Use `X | Y` for unions on Python 3.10+, otherwise `Union[X, Y]` or `Optional[X]`.
3. Be explicit about `None` (e.g., `str | None`).
4. Avoid untyped `Any` in public APIs.

## Step 6: Use modern paths and resources

1. Replace `os.path` juggling with `pathlib.Path`.
2. Use `with` context managers for files, sockets, locks, and anything with acquire/release.
3. Specify `encoding="utf-8"` explicitly for text files.
4. Use f-strings for formatting unless a standard or i18n pipeline requires `%` or `format`.

## Step 7: Format docstrings and names

1. Follow the project convention for docstrings (e.g., Google style sections: `Args:`, `Returns:`, `Raises:`).
2. Use `snake_case` for functions and variables, `PascalCase` for classes, and `UPPER_SNAKE` for constants.
3. Avoid single-letter names except in short scopes.

## References

- Read `references/reference.md` for Pyguide section map and toolchain notes.
- Read `references/combining-skills.md` for combining with maintainable-code.

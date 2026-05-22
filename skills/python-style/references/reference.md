# Reference: Google Python Style Guide and this skill

## Canonical document

**[Google Python Style Guide](https://google.github.io/styleguide/pyguide.html)** — authoritative text; this skill **distills** a subset for agents. Do not paste large excerpts into chat; link here when the user needs the full rule set.

## Why not copy pyguide verbatim

- **Token budget:** Skills should stay concise; the full guide is long and occasionally updated.
- **Tool drift:** Google’s guide still references **pylint** in places; many teams now use **Ruff**. Always defer to the **repository’s** `pyproject.toml`, `ruff.toml`, `mypy.ini`, and CI.
- **Project overrides:** Line length, formatter (Black / Ruff format), and docstring style may differ—follow repo conventions first.

## Section map (pyguide → `python-style` SKILL.md)

| Pyguide area | Covered in SKILL.md as |
|--------------|-------------------------|
| §2.1 Lint | Tooling: Ruff / project linter |
| §2.2 Imports | Imports |
| §2.4 Exceptions | Exceptions |
| §2.12 Default argument values | Mutable defaults |
| §2.21 / §3.19 Type annotations | Typing |
| §3.8 Comments and docstrings | Docstrings |
| §3.10 Strings / logging | Strings, logging, errors |
| §3.11 Files, sockets, resources | Paths, I/O, resources |
| §3.16 Naming | Names and structure |

Sections not duplicated here (still follow pyguide + project if applicable): threading (§2.18), power features (§2.19), exhaustive style nitpicks (§3.2–3.7) when Ruff enforces them.

## Conditional and typing-only imports

Pattern aligned with common practice and pyguide’s typing sections:

```python
from __future__ import annotations

from typing import TYPE_CHECKING

if TYPE_CHECKING:
    import heavy_optional_module  # only for type checkers
```

Use this to break cycles and avoid runtime cost of optional dependencies. For lazy runtime imports (plugins), import inside the function that needs them.

## Type checkers

- **mypy:** strictness flags vary; match `pyproject.toml` `[tool.mypy]`.
- **Pyright / Pylance:** often stricter on `reportUnknown*`; align with VS Code / CI settings.

## Further reading (industry, not duplicated)

- [PEP 8](https://peps.python.org/pep-0008/) — style baseline; Ruff often encodes PEP 8 + extensions.
- [Typing PEPs](https://typing.readthedocs.io/) — `typing` and `typing_extensions` evolution.

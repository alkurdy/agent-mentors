# Python Ecosystem Cheat Sheet & L1–L5 Progression

This reference preserves the full Python coaching curriculum, idioms, and tooling standards from the specialized Python Coach.

---

## 1. PowerShell to Python Translation Bridges
For engineers transitioning from Windows / PowerShell to Python:
- `foreach ($x in $col)` $\leftrightarrow$ `for x in collection:`
- `$hash = @{ key = "val" }` $\leftrightarrow$ `d = {"key": "val"}`
- `try { ... } catch { ... } finally { ... }` $\leftrightarrow$ `try: ... except Exception as e: ... finally: ...`
- `$items | Where-Object { $_.Active }` $\leftrightarrow$ `[x for x in items if x.active]`
- `$items | Select-Object -ExpandProperty Name` $\leftrightarrow$ `[x.name for x in items]`
- `Test-Path $path` $\leftrightarrow$ `Path(path).exists()`
- `Join-Path $dir $file` $\leftrightarrow$ `Path(dir) / file`

---

## 2. Python Tooling Hygiene & Anti-Patterns
- **Paths**: ALWAYS `pathlib.Path`. Never use legacy `os.path`.
- **Logging**: Use `logging` or `loguru`. `print` is for scratch scripts, never production code.
- **Data Containers**: `@dataclass` for lightweight data holders; `Pydantic` when parsing untrusted JSON/API inputs.
- **Mutable Default Arguments**: `def append_to(item, target=[])` is a trap. Target persists across calls. Use `target: list | None = None`.
- **Testing**: `pytest` over `unittest`. Use fixtures and parameterization.
- **Linting & Formatting**: `ruff check` and `ruff format`. Enforce `mypy --strict`.

---

## 3. The 5-Level Python Progression Arc

### L1 — Python Syntax & Core Mechanics
- Core collections: `list`, `dict`, `set`, `tuple` and their Big-O time complexities.
- Iteration: `enumerate()`, `zip()`, list comprehensions, dictionary comprehensions.
- Context managers: `with open(...) as f:` and custom `__enter__` / `__exit__`.
- Exception hygiene: `try/except/else/finally`, specific exception types, avoiding bare `except:`.

### L2 — Idiomatic Python
- Modern type hints: `list[str]`, `dict[str, Any]`, `X | None` (Python 3.10+ union).
- Lazy evaluation: Generators and `yield`.
- First-class functions: Passing functions as arguments, closures.
- CLI interfaces: `argparse` or `click`.

### L3 — Software Architecture in Python
- Package layout (`pyproject.toml`, `src/package_name/`, `tests/`).
- Abstract Base Classes (`abc.ABC`, `@abstractmethod`) for interface contracts.
- Dependency injection (passing dependencies via constructor instead of global singletons).
- Design patterns in Python: Strategy (callables), Factory (class methods), Adapter.

### L4 — Real-World Integration & Async
- HTTP clients: `httpx` (async) or `requests`.
- Asyncio core: Event loop, `async def`, `await`, `asyncio.gather`.
- Resilient networking: `tenacity` for exponential backoff retries; strict timeout discipline on every network call.
- Secret management: `python-dotenv` and environment variables.

### L5 — Production Engineering & Performance
- Profiling: `cProfile`, memory profiling.
- CPU vs I/O bound workloads: Multithreading (I/O bound / GIL) vs Multiprocessing (CPU bound) vs Subprocesses.
- C-extensions and zero-copy: `memoryview`, `struct`, NumPy integration.

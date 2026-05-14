---
name: python-pep8-style
description: >-
  Enforces Python style per PEP 8 with a 132-character line limit (extension of
  PEP defaults). Covers layout, imports, whitespace, naming, comments, public
  APIs, and programming recommendations. Must be read and applied whenever a
  Python source file (.py) is edited, created, or refactored. Also use when
  writing or reviewing Python without file edits, formatting Python, discussing
  style, lint, or PEP 8.
---

# Python style (PEP 8 + 132 columns)

## Authority and full text

- **Official PEP (authoritative):** [PEP 8 – Style Guide for Python Code](https://peps.python.org/pep-0008/)
- **Mirrored extract in this repo:** [reference.md](reference.md) (HTML conversion; prefer the website if they differ)
- **Docstrings (companion):** [PEP 257](https://peps.python.org/pep-0257/)

If a repository has its own style guide, **that guide wins** for that project.

---

## Line length (this skill’s extension of PEP 8)

PEP 8 recommends limiting lines to **79** characters for code and **72** for flowing blocks of text in comments and docstrings, and allows teams to agree on limits up to **99** for code when docstrings/comments stay at 72.

**This skill adopts a single hard limit:**

- **Maximum 132 characters per line** for code, comments, and docstrings, unless breaking earlier improves readability (e.g. narrow prose, tables in comments).
- Still **wrap long expressions** using implied continuation inside `()`, `[]`, `{}` rather than backslashes when possible (PEP 8 preference unchanged).
- Prefer **not** padding or reflowing unrelated code solely to use the full 132 columns.

---

## Introduction (PEP 8)

Conventions target readability and consistency across Python code. The style guide evolves; project-specific rules override this skill.

---

## A foolish consistency

Readability counts (PEP 20). Order of priority: consistency within a function/module > consistency within the project > consistency with PEP 8. Do not break backwards compatibility just to satisfy style.

Good reasons to ignore a guideline include: readability would suffer; matching legacy surrounding code; code predates the rule and is otherwise untouched; older Python versions lack a recommended feature.

---

## Code layout

### Indentation

- Use **4 spaces** per indentation level.
- Continuation lines: align wrapped elements vertically with the opening delimiter inside `()`, `[]`, `{}`, or use a **hanging indent** with no arguments on the first line and extra indent so continuations are visually distinct.
- The 4-space rule is **optional** on continuation lines only; hanging indents may use other indents if clear.
- Long `if` conditions: several styles are acceptable (no extra indent; comment for separation; extra indent on continuation). Closing `)`, `]`, `}` may align with the last line of the list or with the line that starts the construct.

### Tabs or spaces

- **Spaces** are preferred. Tabs only for consistency with existing tab-indented code.
- **Never** mix tabs and spaces for indentation.

### Blank lines

- **Two** blank lines between top-level `def` and `class`.
- **One** blank line between methods inside a class.
- Extra blank lines sparingly between related function groups; omit between tight related one-liners.
- Blank lines inside functions sparingly for logical sections.
- Form feed (`^L`) is legal whitespace; some tools treat it as a page break.

### Source file encoding

- Prefer **UTF-8**; core CPython uses UTF-8 without an encoding cookie.
- Use non-ASCII sparingly; standard library identifiers must be ASCII (see PEP 3131 policy for identifiers).

### Imports

- Usually **one import per line**; `from mod import a, b` is fine when they are symbols from the same module.
- Order: (1) standard library, (2) third party, (3) local; **blank line** between groups.
- **Absolute imports** recommended; explicit **relative** imports acceptable in complex packages.
- Importing classes: `from mymodule import MyClass` unless it causes clashes—then `import mymodule` and `mymodule.MyClass`.
- Avoid `from module import *` except documented re-export of a public API.
- Imports live at the top after module docstring, before globals/constants.

### Module-level dunders

Place `__all__`, `__author__`, `__version__`, etc. after the module docstring and before imports, **except** `from __future__` imports, which must be first after the docstring.

---

## String quotes

Single- and double-quoted strings are equivalent; **pick one rule and stick to it**. If the string contains one quote kind, use the other to avoid escapes. **Triple-quoted strings:** use `"""` for consistency with PEP 257.

---

## Whitespace in expressions and statements

### Pet peeves (avoid extra spaces)

- No space immediately inside `(`, `[`, `{` or before `)`, `]`, `}`.
- No space before comma/semicolon/colon (except slice rules).
- Slices: colon like a binary operator—**equal spaces** on both sides when parameters are present; **no** space when a slice parameter is omitted.
- No space before `(` of call, indexing, or slicing.
- Do not align `=` with spaces across unrelated assignments.

### Other recommendations

- Add spaces around lowest-priority operators when it helps; never more than one space; symmetric around binary operators.
- **Function annotations:** space after `:` in parameters; spaces around `->` if present.
- **Default parameters:** no spaces around `=` for unannotated defaults and keyword calls; **with** annotations, spaces around `=` in the default: `sep: AnyStr = None`.
- Avoid compound one-line `if`/`for`/`while` with large bodies; never chain multi-clause statements on one line.

### Binary operators (line breaks)

For wrapped expressions, **breaking before** the binary operator (Knuth style) is suggested for new code; be **locally consistent** if the codebase uses the other style.

---

## Trailing commas

- Singleton tuple: `FILES = ('setup.cfg',)` — comma required; parentheses recommended.
- When lists/tuples/args may grow, one item per line with trailing comma and closing bracket on the next line is helpful for diffs—**do not** put trailing comma on the same line as the closing delimiter (except singleton tuple case).

---

## Comments

- **Block comments** apply to some or all code that follows; indent to the same level as that code; start each line with `#` + single space unless commenting out code.
- **Inline comments** sparingly; separate from statement by **at least two spaces**; start `#` + single space; avoid stating the obvious; use for non-obvious rationale or warnings.

### Documentation strings

Follow **PEP 257**. Wrap for readability; this skill allows lines up to **132** characters (see Line length above).

---

## Naming conventions

### Overriding principle

Names visible as **public API** should reflect **usage**, not implementation.

### Styles (recognize them)

`CapitalizedWords` (CapWords), `lowercase`, `lower_with_underscores`, `UPPER_CASE`, `mixedCase` (legacy). With acronyms in CapWords, capitalize all letters: `HTTPServerError`.

Leading/trailing underscore conventions:

- `_single_leading_underscore`: internal / not imported by `from M import *`.
- `__double_leading_underscore`: name mangling on class attributes (use sparingly).
- `__double_leading_and_trailing__`: magic names only as documented—never invent.

### Prescriptive rules

- Avoid **`l`**, **`O`**, **`I`** as single-letter names (confusable with `1`, `0`).
- **Stdlib identifiers:** ASCII per PEP 3131 policy.
- **Modules:** short, all lowercase; underscores in module names OK if readability improves; packages lowercase, underscores discouraged. C extension companion: leading underscore (e.g. `_socket`).
- **Classes:** CapWords; callable interfaces may use function naming if documented.
- **Type variables (PEP 484):** CapWords, short names (`T`, `AnyStr`); suffix `_co` / `_contra` for variance.
- **Exceptions:** class naming + suffix **`Error`** when it is an error.
- **Globals (module-internal):** like functions; for `from M import *` modules use `__all__` or underscore prefix for non-public globals.
- **Functions and variables:** lowercase with underscores; `mixedCase` only where dominant legacy style exists.
- **Methods:** first arg **`self`**, class methods first arg **`cls`**. Reserved-word clashes: trailing underscore (`class_`).
- **Methods / instance variables:** like functions; one leading `_` for non-public; `__` prefix to avoid subclass clashes (see PEP for tradeoffs).
- **Constants:** module-level `UPPER_WITH_UNDERSCORES`.

### Designing for inheritance

Decide public vs non-public vs subclass API explicitly. Prefer simple public data attributes; use `@property` when behavior must grow. Avoid expensive work hidden behind property syntax. Use `__` mangling only when avoiding accidental subclass collisions is worth the cost.

### Public and internal interfaces

Only **public** interfaces get backwards-compatibility guarantees. **Documented** names are public unless marked provisional/internal. **Undocumented** names are internal. Set **`__all__`** to declare public API; internal names still use `_` prefix. Imported names are implementation details unless documented as API (e.g. `os.path`).

---

## Programming recommendations (selected)

- Do not rely on CPython-specific refcount optimizations for string concat; use `''.join(...)` in hot paths.
- Compare **`None`** with `is` / `is not`, never `==` / `!=`.
- Prefer **`if foo is not None:`** over **`if not foo is None:`**.
- Implement rich comparisons consistently or use `@functools.total_ordering`.
- Prefer **`def f(x): return 2*x`** over binding a **`lambda`** to a name.
- Derive exceptions from **`Exception`**, not **`BaseException`**, except rare cases. Design hierarchies for what catchers need. Use **`raise X from Y`** / **`from None`** appropriately; preserve context when converting exceptions.
- Catch **`except SpecificError:`**; avoid bare **`except:`** (it catches `KeyboardInterrupt`, `SystemExit`, masks bugs). Use **`except Exception:`** when you truly mean “almost everything”. Keep **`try`** bodies minimal.
- Use context managers via explicit functions when they do more than acquire/release.
- **`return` consistency:** either all branches return a value or none; if any branch returns an expression, use explicit **`return None`** where needed and a final explicit return when reachable.
- Use **`str.startswith` / `endswith`**, not slice equality, for prefixes/suffixes.
- Use **`isinstance`**, not direct `type(...) is` comparisons, for types.
- For sequences, rely on truthiness: **`if seq:`** / **`if not seq:`**, not `len(seq)`.
- Never compare booleans to **`True`/`False`** with `==` / `is`.
- Avoid **`return`/`break`/`continue`** that exit a **`finally`** (suppresses active exceptions).
- **Annotations:** follow PEP 484 syntax; stdlib conservative; optional type checkers; `# type: ignore` at top if needed per PEP 484.
- **Variable annotations:** one space after `:`; no space before `:`; spaces around `=` when RHS present (`label: str = 'x'`).

---

## Function and variable annotations (formatting recap)

- Parameter annotations: space after colon, no space before: `def munge(input: AnyStr): ...`
- Return arrow: spaces around `->` when present: `def munge() -> PosInt: ...`
- Combined with defaults: `def munge(sep: AnyStr = None): ...`

---

## How to use this skill

**Trigger (mandatory):** Whenever the task involves changing or creating a `.py`
file, use this skill for those edits (and keep following it until the Python
changes are done).

1. Apply rules above for edits, reviews, and generated Python.
2. Enforce **132** as the maximum line length unless the project configures otherwise.
3. For wording, examples, or edge cases not covered here, read **reference.md** or the [live PEP 8](https://peps.python.org/pep-0008/).

---

## References (PEP 8)

PEP 8 cites PEP 7 (C style), Barry’s guide, Knuth, PEP 207, 3151, typeshed, etc. See the **References** section on the official page and footnotes in [reference.md](reference.md).

# GDR's C++ Coding Guidelines

These rules were primarily written for coding agents that I work with.


For anything not explicitly covered here, refer to the [C++ Core Guidelines](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md).

## Standard version

* Use Contemporary C++ (e.g. at minimum C++23)

## Facilities

* Use standard facilities where available, e.g. `std::format`, `std::jthread`, `std::ranges`, structured bindings, concepts, etc.


## Naming


| Elemeny | Style | Example |
|---------|-------|---------|
|Types (classes, structs, enums) | `PascalCase` | `Elaboration`, `AlgebraSort` |
| Enumerators | `PascalCase` | `Identifier`, `NonZero` |
| Functions, variables, parameters, | `snake_case` | `read_file`, `emit_insn` |
| Filenames | lowercase with hyphens | `bemol-runtime.cxx` |
| Modules, partitions | `PascalCase` | `MemoryMapped`, `:IO` |  

## File extensions

* Module interfaces: `.ixx`
* Source files: `.cxx`
* Header files: `.hxx`

## Include order

Include groups are separated by blank lines in this order:
1. **standard library headers** -- `<string>`, `<algorithm>`, etc.
2. **third-party headers** -- `<gsl/narrow>`, `<ipr/interface>`, etc.
3. **project headers** -- `"bemol/function.hxx"`, `"bemol/ir.hxx"`, etc. 

Standard library headers or modules always come first.

## Brace style

* **Allman** for function definitions and control-flow blocs (`if`, `for`, `while`, `switch`).  Opening `{` on a new line.
* **K&R** for type definitions (`struct`, `class`, `enum`) and namespace definitions.  Opening `{` on the same line as the type or namespace name.
* All explicit control tranfer statements (`return`, `break`, `continue`, `goto`, `throw`, `co_yield`, `co_return`) must be on their own line -- never on the same line as a condition. Exception: trivial inline bodies `{ return true; }`.
* If the control statement is the only statement in a compound statement, the enclosing braces may be omitted.

## Condition expressions

* When testing for a pointer nullness, compare explicitly against `nullptr`, e.g. `ptr == nullptr` or `ptr != nullptr`, rather than relying on implicit boolean conversion.

* Use the alternative logical operator tokens, e.g. `not`, `and`, `or`.  They are standard C++, and read more clearly in complex conditions.


## Function declarations

* Prefer classic function syntax (`T f(...)`) over trailing-return syntax (`auto f(...) -> T`) unless the return type depends on the parameters (e.g. dependent return type).
* Avoid `bool` function parameters.  Prefer a dedicated 2-value `enum class` with `bool` as the underlying type.  This makes call sites self-documenting.


## Virtual override

* Each overrider must have either `override` or `final`
* Use `override` only when extension points are intentional
* Otherwise, prefer `final` to close further overriding in in derived classes.

## Struct vs class

* Use `struct` for type definitions.  The default public access avoids the syntactic ceremony of `class` followed by `public:`.
* Keep `private:` sections whenever data abstraction or invariant protection adds architectural value.
* Forward declarations use `struct` to match the definition.


# Type aliases vs strong enumerations

* Where possible, prefer a strong enumeration with a specified underlying type over a type alias for the same integer type.  This prevents implicit conversions, gives the type its own name in diagnostics, and makes values self-documenting.

## Error handling

* Prefer C++ exceptions when they make code more robust and readable over plumbing `std::expected` through deep call chains.
* Use `std::expected<T, E>` when it is the clearest fot for local, anticipated, recoverable outcomes (especially when explicit branching is part of the API contract).
* Maintain exception safety guarantees, and ensure no leaks or partially-initialized shared state on failure.

## Indentation

* Fully indent declarations inside namespace bodies (4 spaces).
* Consistent 4-space indentation throughout.

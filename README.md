# Gleam Code Generator for `aleph-syntax-tree`

This crate provides a code generator that transforms an abstract syntax tree (AST) from the [`aleph-syntax-tree`](https://github.com/aleph-lang/aleph-syntax-tree) crate into [Gleam](https://gleam.run/) source code.

## Features

- Converts `AlephTree` nodes into Gleam source code
- Supports common language constructs:
  - Literals: `Int`, `Float`, `Bool`, `String`, `Bytes`
  - Data structures: `Array`, `Tuple`
  - Control flow: `If`, `Let`, `LetRec`, `Match`
  - Boolean and arithmetic operations: `And`, `Or`, `Add`, `Sub`, `Mul`, `Div`, `Eq`, `LE`, `In`
  - Pattern matching with `case` expressions
  - Comments, assertions, simple imports
- Skips unsupported nodes like `Class`, `While`, `Put`, `Remove`, or complex constructs

## Usage

### Add to your project

Ensure you depend on the [`aleph-syntax-tree`](https://github.com/aleph-lang/aleph-syntax-tree) crate.

### Example

```rust
use aleph_syntax_tree::syntax::AlephTree as at;
use your_crate::generate;

fn main() {
    let ast = at::Let {
        var: "answer".into(),
        is_pointer: false,
        value: Box::new(at::Int { value: "42".into() }),
        expr: Box::new(at::Return {
            value: Box::new(at::Var { var: "answer".into(), is_pointer: false }),
        }),
    };

    let gleam_code = generate(ast);
    println!("{}", gleam_code);
}
```

### Output

```gleam
let answer = 42
answer
```

## Function Signature

```rust
pub fn generate(ast: AlephTree) -> String
```

Returns the generated Gleam code as a `String`.

## Limitations

- Does not support class structures, `Put`, `Remove`, or `While` loops
- `Let` expressions are line-based, no expression wrapping
- Match and function definitions are translated using simple `case` and `pub fn` formats
- Pattern matching does not include type destructuring


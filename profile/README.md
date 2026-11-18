<div align="center">

<img src="logo.webp" alt="March logo" width="140">

# March

[Website](https://march-lang.org/) · [Tour](https://march-lang.org/docs/tour/) · [Playground](https://march-lang.org/docs/playground/) · [Install](https://march-lang.org/docs/installation/) · [Standard library](https://march-lang.org/docs/stdlib/)

</div>

March is a statically typed functional language. It has ML-style algebraic data types and
pattern matching, Erlang-style actors and supervision, and compiles to native code with LLVM.
Memory is reference counted ([Perceus](https://march-lang.org/docs/memory-model/)), so there's
no garbage collector.

```march
mod Main do
  needs IO.Console

  type Shape = Circle(Float) | Rect(Float, Float)

  fn area(s : Shape) : Float do
    match s do
      Circle(r)  -> 3.14159 *. r *. r
      Rect(w, h) -> w *. h
    end
  end

  actor Ledger do
    state { total : Float }
    init  { total: 0.0 }

    on Record(s : Shape) do
      let total = state.total +. area(s)
      println("total area: " ++ float_to_string(total))
      { state with total: total }
    end
  end

  fn main(cap : Cap(IO.Console)) : () do
    let ledger = spawn(Ledger)
    send(ledger, Record(Circle(1.0)))
    send(ledger, Record(Rect(2.0, 3.0)))
    run_until_idle()
  end
end
```

The `needs` line and the `Cap(IO.Console)` parameter on `main` limit what the program can do.
If anything it calls, a library included, tries to open a file or a socket, it won't compile.
The docs have more on [capabilities](https://march-lang.org/docs/capabilities/),
[linear types](https://march-lang.org/docs/linear-types/) and
[refinement types](https://march-lang.org/docs/refinement-types/).

## Try it

Use the [playground](https://march-lang.org/docs/playground/) in your browser, or install
`march` and the `forge` build tool:

```sh
curl -fsSL https://raw.githubusercontent.com/march-language/march/main/install.sh | sh
```

Then see [Getting started](https://march-lang.org/docs/getting-started/).

## Repositories

**Language**

- [march](https://github.com/march-language/march): compiler, runtime, standard library, `forge` and the language server
- [march-lean](https://github.com/march-language/march-lean): Lean 4 proofs about the type system and Perceus/FBIP
- [setup-march](https://github.com/march-language/setup-march): GitHub Action that installs March in CI
- [march-language.github.io](https://github.com/march-language/march-language.github.io): the website

**Libraries**

- [bastion](https://github.com/march-language/bastion): web framework
- [depot](https://github.com/march-language/depot): database access and data mapping
- [conduit](https://github.com/march-language/conduit): background jobs

**Tools**

- [march_doc](https://github.com/march-language/march_doc): documentation generator
- [scroll](https://github.com/march-language/scroll): notebooks stored as plain Markdown
- [sigil](https://github.com/march-language/sigil): static site generator

## Contributing

File issues and pull requests in the repo they're about. Anything to do with the language or
compiler goes to [march-language/march](https://github.com/march-language/march/issues).

# [The 'Explainable' codebase](https://github.com/smucclaw/dsl/tree/main/lib/haskell/explainable)

## Historical Context

YM: Meng had written an embedded DSL during the insurance usecase, and wanted something like that that could compute things and give traces for its computations, and crucially, do it fast (recall that he did not like how the Logical English backend had been slow in its handling of requests).

These components were intended to:

- illustrate an evaluation-tree approach to logging computation for explainability purposes; and
- provide a parallel implementation of infrastructure to support the insurance use case in 2023.

While the primary codepaths for the insurance use case ran with L4 and
Logical English, this Explainable codebase supported [rapid
experimentation with alternative
formulations](https://github.com/smucclaw/usecases/blob/b256ffb78d21f15335d789352b5e5da957e38e35/sect10-haskell/src/ToMathLang.hs#L334-L370),
and were used in "cleanroom testing" of the primary codebase.

See also:

* <https://github.com/smucclaw/usecases/blob/main/sect10-typescript/>
* (./generic_mathlang.md)
* (./mathlang.md)

Interestingly, this codebase features twin implementations in Haskell and Typescript, with working runtimes in both languages.

We wanted a Typescript runtime so that the computations could run in the end-user's browser.

Links to Meng's embedded DSL:

- [The Haskell embedded DSL](https://github.com/smucclaw/usecases/blob/main/sect10-haskell/src/ToMathLang.hs)
  - For comparison: [Natural L4 version of the rules PAU0 and PAU4](https://docs.google.com/spreadsheets/d/1cWAb7Ba4HJovQn1PquZzYJjnjKUuhEPhNHzAH4ZfV4I/edit#gid=2100528279)
- [The Haskell types and runtime](https://github.com/smucclaw/dsl/blob/main/lib/haskell/explainable/src/Explainable/MathLang.hs)
- [The TS output pretty-printed from Haskell)](https://github.com/smucclaw/usecases/blob/main/sect10-typescript/src/pau.ts)
- [The TS runtime which evaluates the TS output](https://github.com/smucclaw/usecases/blob/main/sect10-typescript/src/mathlang.ts)

That embedded DSL should give you some sense for what Meng has in mind with his 'MathLang'.

The Explainable codebase includes the following components.

- [MathLang](./mathlang.md): a DSL for arithmetic expressions and predicates
- Explainable: a monad for evaluating MathLang expressions and providing explanations for the (intermediate and final) results

## Use

In 2023, the "business logic" of the use case was independently encoded in Haskell and then pretty-printed to Typescript for evaluation.

``` mermaid

graph TB;

    classDef tsruntime fill:#9ff,stroke:#333;

    A1["ToMathLang.hs\n(usecases/sect10-haskell/src/)"]

    A1 -- "imports" --> B;
    B["Explainable/MathLang.hs"]

    A1 -- "defines" --> A2["user-space decision logic\n(in internal DSL)"]

    A2 -- "has type" --> A3["Explainable.MathLang.Expr"];

    A3 -- "can be natively evaluated by" -->B

    B -- "pretty-prints Exprs to" -->C["Typescript output\n(sect10-typescript/src/\npau.ts)"]

    C -- "imports" -->D["mathlang.ts\n(sect10-typescript/src/\nmathlang.ts)"]

    F["The Abstract MathLang Language"]
    D -- "implements\nin Typescript" -->F
    B -- "implements\nin Haskell"    -->F

    C -- "imported by" -->G["crunch.ts\n(sect10-typescript/src/crunch.ts)"]

    G -- "outputs to" -->H["sect10-typescript/\ntests/\ndot/"]

    class C,G,H,D tsruntime
```

All the above is orchestrated by a `Makefile` under `sect10-typescript`.

Note that the business logic was independently encoded in the internal MathLang DSL and was not wired up to read from the Natural4 spreadsheets. As a clean-room re-implementation, that's fine, but in the long run, the [DRY principle](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself) suggests that future MathLang toolchains should read from the spreadsheet or whatever natural4 encoding is canonical, rather than a reimplementation in `ToMathLang.hs`.

### 'Generic MathLang' vs 'MathLang'

The term 'MathLang' is used in a couple of different ways. On the broadest sense, MathLang is supposed to be some kind of functional programming language. But there are also more specific senses; for example, it could also refer to *the specific embedded DSL Meng had whipped up* (see above) --- a DSL that departs from a more 'generic' / 'undergrad-textbook' functional programming language in some ways that make the translation to it more effortful.

It is in this context that 'Generic MathLang' should be understood. Because the specific embedded-in-Haskell DSL that Meng had whipped up was

1. was written in a way that was hard to understand (I think Joe has been working on improving the code quality, though) and

2. departs from a more 'generic' / 'undergrad-textbook' functional programming language in some ways that make the translation to it more effortful --- indeed, *unnecessarily* effortful, given the ostensible mandate ('get some kind of lambda calculus based thing out asap')

YM decided to first translate the output from the Natural L4 parser to an intermediate representation that he called *Generic MathLang* --- an intermediate representation that amounted to more generic, **un**typed lambda-calculus-looking AST --- before further translating it to other formats. That is, the envisioned pipeline was something like

```ascii
Natural L4 parser => Generic MathLang => (e.g.) serialization of Generic MathLang for an in-browser interpreter | Meng's MathLang | ...etc
```

Doing this allows us, not only to (in principle) get a lot more quickly to a working web form backed by an in-browser interpreter (because, again, serializing Generic MathLang and then writing an in-browser interpreter with tracing would be a lot faster than having to read and test Meng's code carefully), but also improves *code reuse*, since we now have a lambda-calculus-y intermediate representation that we can work from if we need to experiment with producing other MathLang variants.

YM had started implementing this over Dec 2023 - Jan 2024, but subsequently passed the baton on to Inari.

This was admittedly YM's own understanding of Generic MathLang, but [YM sees that Inari seems to also have stuck to this understanding of Generic MathLang; see her docs on the Generic MathLang codebase.](./generic_mathlang.md)

YM does not fully understand the following remark by Meng --- it's not clear why it's relevant that spreadsheets are 'the single source of truth', especially since the MathLang dialect does not differ from the other current dialects of L4 with regards to this --- but maybe others would be able to understand it.

> In 2024, we built a Generic MathLang component to serve as a bridge between the Natural4 codebase and the `MathLang.hs` codebase, so the spreadsheets could serve as the single source of truth.

### Meng's diagram of the architecture

``` mermaid
graph TB;
    A1["Google Sheets tab\n(via API)"] -- "L4/Refresh" --> C;
    A2["command-line invocation\n(perhaps involving fswatch)"] -- calls --> C;
    classDef highlight fill:#f9f,stroke:#333,stroke-width:2px;

    subgraph C ["natural4-exe (app/Main.hs)"]
        Parser --> Interpreter;
    end

    C --"runs"--> C1["the Generic MathLang codebase\n(2024)"];

    C1--"imports"-->D[["LS/XPile/MathLang"]];
    C1--"imports"-->F

    D--"transpiles to"-->E[("workdir/uuid/\nmathlangGen\nmathlangTS")];
    class C1,D,E highlight;

    classDef tsruntime fill:#9ff,stroke:#333;

    E--"hopefully somehow\nbackward-compatible with"-->F["Explainable/MathLang.hs"];

    F --"which takes it the rest of the way to"-->G["the Typescript runtime"]
 class F,G tsruntime
```

## Types

```haskell
type ExplainableIO  r st a = RWST         (HistoryPath,r) [String] st IO (a,XP)
type Explainable    r st a = ExplainableIO r st a
```

## Evaluation

See the [Explainable README](https://github.com/smucclaw/dsl/tree/main/lib/haskell/explainable#readme).

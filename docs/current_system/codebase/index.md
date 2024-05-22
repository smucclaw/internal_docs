# Notes on the Codebase


## [The Natural L4 codebase](https://github.com/smucclaw/dsl/tree/main/lib/haskell/natural4)

### AST / CST

* [The `Rule` data structure](./rule_ast.md)

### The Natural L4 parser

The first-generation parser was based on BNFC: see
- https://github.com/smucclaw/baby-l4/blob/main/l4.bnfc
- https://bnfc.digitalgrammars.com/

This work was done around 2020, 2021. It parsed text-file input.

The second-generation parser for the spreadsheet syntax was based on Megaparsec: see
- https://github.com/smucclaw/dsl/blob/main/lib/haskell/natural4/src/LS/

The monadic parser is slow. Profiling it with a flame graph shows that
a great deal of time is spent backtracking. The parser does a bunch of
lookahead and other work to deal with indentation. BNFC has native
support for "layout rule" logic. In this parser, we hacked up an emulation of indentation-as-parenthesis, which doesn't work very well.

[Many test
cases](https://github.com/smucclaw/dsl/blob/main/lib/haskell/natural4/test/Parsing/megaparsing/)
later, we still don't have full confidence in the parsing. For
example, trying to set up a `BoolStructR` that has a non-null `PrePost`
label can sometimes fail unintuitively when there is too much space or
not enough space between the label and the children.

The parser also hoists inline rules into top-level rules. This is the purpose of operating the parser within a Writer monad. See [tellIDFirst](https://github.com/smucclaw/dsl/blob/main/lib/haskell/natural4/src/LS/Tokens.hs#L1035-L1036).

### Meng's Analyzer / 'Interpreter'

After the input CSV is parsed into a collection of `Rule` types,
[Interpreter.hs](https://github.com/smucclaw/dsl/blob/main/lib/haskell/natural4/src/LS/Interpreter.hs)
attempts to analyze and reorganize it, to make it more ready for the
various transpilers to export from.

This is where rule substitution happens and simple rule rewriting/optimization.

This module is not really an interpreter. It should have been called an Analyzer.


```mermaid
graph TB;
    classDef natural4exe fill:#f9f,stroke:#333,stroke-width:2px;

    A["Google Sheets tab"] -- "L4/Refresh" --> B[["Apps Script Sanic Hello.py"]];
	B -- calls -->C;
    subgraph C ["natural4-exe (app/Main.hs)"]
    parser --> interpreter;
    end
	
	C --"runs"--> C1["the Purescript and Vue codebase\n(2021/2022)"];
	C1--"imports"-->D[["LS/XPile/Purescript.hs"]];
    D--"transpiles to"-->E[("workdir/uuid/\npurs/LATEST.purs")];

    C --"runs"--> G0["the JSON Schema transpiler\n(2023)"];
	G0--"imports"-->G1[["LS/XPile/ExportTypes.hs"]];
	G1--"transpiles to"-->G2[("workdir/uuid/\njsonTp/LATEST.json")];
```


### Transpilers

#### To Prolog (Prolog.hs)

* Status: Deprecated
* Context: According to Meng (16 May), he had written this as a way of thinking through the semantics of L4. The thought was apparently to get a translational semantics for L4 by working out what the corresponding Prolog should be.
* YM: In any case, even if you wanted to go to Prolog, the Logical English transpiler would probably be a better way of doing that.

#### Logical English

I'll briefly note some in-the-weeds decisions about the implementation here (see [the system overview](../index.md) for a more high-level discussion). 

Re Meng's Analyzer / 'Interpreter':
* This does not use the output from Meng's Analyzer / 'Interpreter'. Instead, it starts from the output from the parser. 
* This is because 
  * (i) it wasn't clear that the semantics that was implicit in Meng's Analyzer / 'Interpreter' was something that would be compatible with that for this fragment of L4.
  * (ii) it looked from a quick glance like some of the transformations that the analyzer/interpreter was doing might be lossy, e.g. there's some substitution or inlining
  * (iii) it wasn't clear what the specification for Meng's Analyzer / 'Interpreter' was; and in particular, what assumptions and guarantees it was making or providing.

It's also worth mentioning that there's a golden test framework in `dsl/lib/haskell/natural4/test/LS/XPile/LogicalEnglish` that Joe and Jo Hsi ahd worked on, and that we found helpful when developing this transpiler.

Finally, note that some of the comments in the LE transpiler codebase are out of date / out of sync with the code. Apologies about that --- I (YM) will try to clean that up soon.

## [The 'Explainable' codebase](https://github.com/smucclaw/dsl/tree/main/lib/haskell/explainable)

The Explainable codebase includes the following components.

- [MathLang](./mathlang.md): a DSL for arithmetic expressions and predicates
- [Explainable](./explainable.md): a monad for evaluating MathLang expressions and providing explanations for the (intermediate and final) results

[TODO-Meng]

## Visualizations

### [Ladder Diagram](https://github.com/smucclaw/ladder-diagram)

* [The main Ladder Diagram repo](https://github.com/smucclaw/ladder-diagram)
  * There are also docs available for this, via
  ```
  npm install jsdoc -g
  npm run docs
  ```
* [Specification in Google Drive](https://drive.google.com/drive/folders/1y7TssfA925VuyuAt8VBaNxlRTo8KyqlS?usp=sharing)

### Petri Net stuff

According to Meng (16 May 2024):

* Context: This was meant to be a visualization for the state transition parts / deontics-handling parts of L4.
* Status: Although we still want to offer visualizations for the state transition-y parts in the next iteration of L4,  we don't need to stick to Petri Nets. We are free to move to something else, if there are better alternatives.

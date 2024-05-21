# Notes on the Codebase


## [The Natural L4 codebase](https://github.com/smucclaw/dsl/tree/main/lib/haskell/natural4)

### AST

* [The `Rule` data structure](./rule_ast.md)

### Meng's Analyzer / 'Interpreter'

TODO

### Transpilers

#### To Prolog (Prolog.hs)

* Status: Deprecated
* Context: According to Meng (16 May), he had written this as a way of thinking through the semantics of L4. The thought was apparently to get a translational semantics for L4 by working out what the corresponding Prolog should be.
* YM: In any case, even if you wanted to go to Prolog, the Logical English transpiler would probably be a better way of doing that.

## [The 'Explainable' codebase](https://github.com/smucclaw/dsl/tree/main/lib/haskell/explainable)

The Explainable codebase includes the following components.

- [MathLang](./mathlang.md): a DSL for arithmetic expressions and predicates
- [Explainable](./explainable.md): a monad for evaluating MathLang expressions and providing explanations for the (intermediate and final) results

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
# Notes on the Codebase


## [The Natural L4 codebase](https://github.com/smucclaw/dsl/tree/main/lib/haskell/natural4)

### AST

* [The `Rule` data structure](./rule-ast.md)

### Meng's Analyzer / 'Interpreter'

TODO

### Transpilers

#### To Prolog (Prolog.hs)

* Status: Deprecated
* Context: According to Meng (16 May), he had written this as a way of thinking through the semantics of L4. The thought was apparently to get a translational semantics for L4 by working out what the corresponding Prolog should be.
* YM: In any case, even if you wanted to go to Prolog, the Logical English transpiler would probably be a better way of doing that.

## [The 'Explainable' codebase](https://github.com/smucclaw/dsl/tree/main/lib/haskell/explainable)

[TODO]

## Visualizations

### [Ladder Diagram](https://github.com/smucclaw/ladder-diagram)

* [The main Ladder Diagram repo](https://github.com/smucclaw/ladder-diagram)
  * There are also docs available for this, via 
  ```
  npm install jsdoc -g
  npm run docs
  ```
* [Specification in Google Drive](https://drive.google.com/drive/folders/1y7TssfA925VuyuAt8VBaNxlRTo8KyqlS?usp=sharing)

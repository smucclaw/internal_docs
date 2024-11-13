# Notes on the Codebase

Our codebase is split across multiple repos, and even within the `dsl` repo, across multiple `stack` projects under `lib/haskell/`.

The major projects are:

- [anyall](./anyall.md) a foundational representation of boolean structures (conjunctive and disjunctive lists + negation) augmented with labels
- [explainable](./explainable.md) supports verbose evaluation of arithmetic and boolean expressions;  discussion of 'MathLang' is also housed here
- [natural4](./natural4.md) the compilation toolchain for the spreadsheet-based version of the L4 CNL
- [usecases](https://github.com/smucclaw/usecases) contains most of the work specific to the 2023 insurance case.

We also talk about

- [visualizations](./visualizations.md) of boolean and arithmetic "mathlang" structures, and of state transitions as a petri net

Please see the [system architecture map](./architecture.md) to see how these code components work together.

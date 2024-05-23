# Visualizations

A handful of components produce visualizations of L4 expressions.

## [Ladder Diagram](https://github.com/smucclaw/ladder-diagram)

This draws the interactive, clickable diagrams in the web interview.

* [The main Ladder Diagram repo](https://github.com/smucclaw/ladder-diagram)
  * There are also docs available for this, via
  ```
  npm install jsdoc -g
  npm run docs
  ```
* [Specification in Google Drive](https://drive.google.com/drive/folders/1y7TssfA925VuyuAt8VBaNxlRTo8KyqlS?usp=sharing)

### See also the SVGLadder.hs in [AnyAll](./anyall.md).

## Petri Net stuff

https://github.com/smucclaw/dsl/blob/main/lib/haskell/natural4/src/LS/XPile/Petri.hs

According to Meng (16 May 2024):

* Context: This was meant to be a visualization for the state transition parts / deontics-handling parts of L4.
* Status: Although we still want to offer visualizations for the state transition-y parts in the next iteration of L4,  we don't need to stick to Petri Nets. We are free to move to something else, if there are better alternatives.

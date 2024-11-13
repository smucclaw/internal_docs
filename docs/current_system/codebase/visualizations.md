# Visualizations

A handful of components produce visualizations of L4 expressions.

## simple ladder SVGs

In 2020, simple Boolean-only decision logic was visualized with a library inspired by ladder logic Boolean circuit diagrams. See the [original specification in Google Drive](https://drive.google.com/drive/folders/1y7TssfA925VuyuAt8VBaNxlRTo8KyqlS?usp=sharing)

The tiny versions show up in the sidebar and look like this.

![tiny v1 ladder diagram svg](../vis-aasvg-qualifies-tiny.svg)

The fuller versions contain text and look like this.

![full v1 ladder diagram svg](../vis-aasvg-qualifies-full.svg)

See also `SVGLadder.hs` in [AnyAll](./anyall.md)

## interactive ladder HTML

Subsequently, interns Jules and Zeming wrote an interactive version in HTML.

This draws the interactive, clickable diagrams at the bottom of the web interview.

* [smucclaw/ladder-diagram repo](https://github.com/smucclaw/ladder-diagram)
  * There are also docs available for this, via

  ```bash
  npm install jsdoc -g
  npm run docs
  ```

* [Specification in Google Drive](https://drive.google.com/drive/folders/1y7TssfA925VuyuAt8VBaNxlRTo8KyqlS?usp=sharing)

The AST here is formed by the `Circuit` type, which can be a `BoolVar`, `AllQuantifier`, or `AnyQuantifier`.

## expression tree explorer

Our redoubtable interns further wrote code to fold (show/hide) subexpressions of MathLang:

<https://github.com/smucclaw/usecases/tree/mathlang-vis-horizontal>

[Video demo](https://smucclaw.slack.com/files/U057B2P9XT2/F06CN807NC8/vid_20231230_224443_209.mp4)

To try it out and click through the graph and fill in the questions:

```bash
npm i
npm run watch-ts
npm run start
```

The program being visualised is represented as a [`NodeTemplate`](https://github.com/smucclaw/usecases/blob/mathlang-vis/mathlang-vis/ts/index.ts#L43-L47) tree. The `MathlangVis` class renders this tree by converting each `NodeTemplate` into HTML elements.

## Petri Net stuff

<https://github.com/smucclaw/dsl/blob/main/lib/haskell/natural4/src/LS/XPile/Petri.hs>

According to Meng (16 May 2024):

* Context: This was meant to be a visualization for the state transition parts / deontics-handling parts of L4.
* Status: Although we still want to offer visualizations for the state transition-y parts in the next iteration of L4,  we don't need to stick to Petri Nets. We are free to move to something else, if there are better alternatives.
* Prior Art: one of the earliest papers in the field of computable contracts, [Ronald Lee 1998](https://drive.google.com/file/d/1K8bZP8GphlT_YX7kR-nadVwcxNHMswTe/view?usp=drive_link), chose the Petri Net formalism.

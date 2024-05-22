# [The AnyAll codebase](https://github.com/smucclaw/dsl/tree/main/lib/haskell/anyall)

This codebase was carved out and implemented initially by [johsi-k](https://github.com/orgs/smucclaw/people/johsi-k).

It was meant to support boolean structures ("BoolStructs"), augmenting
the basic "and/or", "any/all" operators over lists with some natural
language decorations ("PrePost labels").

It grew to support ["ladder logic" visualization](https://drive.google.com/drive/folders/1y7TssfA925VuyuAt8VBaNxlRTo8KyqlS).

What draws the diagrams?

Well, it depends which diagram you mean. There are two families of diagrams.

The original SVG ladder diagrams that show up in the sidebar are drawn by `SVGLadder.hs`. The sidebar shows tiny versions. They zoom to full-scale versions.

Then there's another family of diagrams, used in the web-app
interview. There, the clickable ladder diagrams that show up at the
bottom of the page are drawn by the `ladder-diagram` repo described
below.

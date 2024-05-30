# Overall 2023 System Architecture Diagram

```mermaid
graph TB;
    classDef natural4exe fill:#f9f,stroke:#333,stroke-width:2px;

    A["Google Sheets tab"] -- "L4/Refresh" --> B[["Apps Script Sanic Hello.py\nsmucclaw/gsheet/natural4-server/""]];
    B -- calls -->C[["natural4-exe (app/Main.hs)"]];

    C --"runs"--> C1["the Purescript and Vue codebase\n(2021/2022)"];
    C1--"imports"-->D[["LS/XPile/Purescript.hs"]];
    D--"transpiles to"-->E[("workdir/uuid/\npurs/LATEST.purs")];
    E--"consumed by"-->F["vue-json frontend\n(smucclaw/vue-pure-pdpa)"];

    C --"runs"--> G0["the JSON Schema transpiler\n(2023)"];
    G0--"imports"-->G1[["LS/XPile/ExportTypes.hs"]];
    G1--"transpiles to"-->G2[("workdir/uuid/\njsonTp/LATEST.json")];
    G2--"consumed by"-->G3["react frontend\n(smucclaw/usecases/smu)"]

    subgraph H1 ["svg generator"]
      H1A[["LS/XPile/SVG.hs"]];
      H1A--"imports"-->H1B[["AnyAll.makeSvg\n(dsl/lib/haskell/anyall)"]];
    end

    C --"runs"--> H0["the simple Boolean circuit visualizer"]
    H0--"imports"--> H1
    H1--"transpiles to"-->H2[("workdir/uuid/\naasvg/LATEST/*.svg")];
    H2--"consumed by"-->H3["spreadsheet sidebar\n(smucclaw/gsheet)"]

    F --"becomes"--> F1["javascript data structure\nQoutJS"]
    F1--"consumed by"-->F2["LadderDiagram.vue"]
    F2--"imports"-->F4["the interactive ladder diagram generator\nladder-diagram js library\n/src/smucclaw/ladder-diagram"]

    C --"runs"-->J1["Logical English transpiler"]
    J1 --"imports"-->J2["LS/XPile/LogicalEnglish.hs"]

    J2--"transpiles to"-->J3

    subgraph J3 ["logical english format"]
        J3a[("workdir/uuid/\nlogical_english/LATEST.le")]
        J3a--"copied to"-->J3b[("smucclaw/usecases/smu/\npublic/le/program.le")]
    end

    J3--"consumed by"-->J4["LE wrapper"]
    J4--"passed once for each query against"-->J5["Logical English Prolog Pengines server"]
    G3--"calls"-->J5

    C --"transpiles to"-->K["Prolog\n(obsolete)"]
    C --"transpiles to"-->L["future targets\nPython API Backend\nJavascript for browser-side\nGo libraries"]
    C --"transpiles to"-->Z["others\n[TODO]"]

```

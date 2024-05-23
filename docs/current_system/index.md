# Overview of the current system

As [the current user-facing documentation puts it](https://l4-documentation.readthedocs.io/en/stable/docs/links-returning-users.html),

> while there is one generic L4 syntax, L4 really admits of different fragments, each with their own specialized semantics, corresponding to the various transpilers. 

We'll systematically discuss each of these transpilers and their statuses when we [discuss the codebase in more detail](./codebase/index.md). But before doing that, let's briefly survey what I (YM) take to be the most currently functional components of the L4 ecosystem --- or, to put it another way, what one can currently do with L4.

## Web form generation

One useful thing you can do with L4 is to scaffold a web form app from an L4 specification.

There have been two generations of this app builder.

```mermaid
graph TB;
    classDef natural4exe fill:#f9f,stroke:#333,stroke-width:2px;

    A["Google Sheets tab"] -- "L4/Refresh" --> B[["Apps Script Sanic Hello.py"]];
	B -- calls -->C[["natural4-exe (app/Main.hs)"]];

	C --"runs"--> C1["the Purescript and Vue codebase\n(2021/2022)"];
	C1--"imports"-->D[["LS/XPile/Purescript.hs"]];
    D--"transpiles to"-->E[("workdir/uuid/\npurs/LATEST.purs")];

    C --"runs"--> G0["the JSON Schema transpiler\n(2023)"];
	G0--"imports"-->G1[["LS/XPile/ExportTypes.hs"]];
	G1--"transpiles to"-->G2[("workdir/uuid/\njsonTp/LATEST.json")];
```

### Propositional-logic-only decision support web app (the Purescript and Vue codebase)

The first generation builds a Vue web app that allows users to answer YES / NO questions to arrive at some sort of decision, and to see a visualization of that (see the discussion of ladder diagrams [TODO -- add links]). The most involved example of this involved making such an app from an encoding of the Personal Data Protection Act. 

This is quite limited in its functionality: it only handles propositional logic.

This app was internally titled "Dolora, the Law Explorer".

#### Historical Context

This was motivated by a 2021/2022 use case around the Personal Data Protection Act.

#### Status 

Still forms part of current demos; badly needs to be superseded.

#### Visible at

1. spreadsheet sidebar, at top.
2. A static snapshot of the generated app is stable and available at https://smucclaw.github.io/mengwong/pdpa

### More sophisticated (arithmetic + dates + some abductive queries) decision support web app (the JSON Schema transpiler)

[TODO: Reorganize and explain what this is first]

The second generation tried to separate MVC layers by using an approach based on [react-jsonschema-form](https://github.com/rjsf-team/react-jsonschema-form) / [vue-form-json-schema](https://github.com/jarvelov/vue-form-json-schema).

[The relevant docs](https://github.com/smucclaw/documentation/blob/main/docs/webform.rst) explain how the web form generation works in some detail. [The JSON schema transpiler docs](https://github.com/smucclaw/documentation/blob/main/docs/transpilers-json-schema.rst) are also relevant.

#### Historical Context

This was motivated by that use case with the insurance company. 

#### Status 

Still relevant, 

* though YM thinks that there's quite a bit that could be improved, especially with regards to the interfaces (in the software design sense). 
* And ideally, we'd also want to re-examine the semantics of the schema definition constructs.
* The JSON Schema transpiler also needs some work. [TODO: Add more detail here on what kind of work]

**NOTE:** The [example form app repo](https://github.com/smucclaw/example-l4-form-app) needs some work. Haven't really bothered polishing it because we'll probably want to improve the web app generation system in more fundamental ways.

### The Logical English transpiler

**TODO: Add discussion of what this is and how this is related to the web app stuff**

[Docs for this can be found on the user-facing doc site](https://github.com/smucclaw/documentation/blob/main/docs/transpilers-logical-english.rst)

There are some ways in which the code for the transpiler could be cleaned up. But that work has been deferred for now, because it's not clear we want to be using this, going forward.

#### Historical Context

##### Motivation

This was motivated by that use case with the insurance company. 

If memory serves me right, we went for this because

* we thought that some form of logic programming would make for a good foundation for the system --- eg, it would be simple to extend it with facilities for abductive reasoning. (You could of course also do this with more work in Haskell.) And some of the designers/implementers of this dialect of L4 had a strong interest in logic programming.

* Logical English was especially convenient to build upon because it is a kind of CNL wrapper of Prolog -- it's basically Prolog with a natural-language-y facade. In that way, Logical English made it a lot easier to go from a CNL like Natural L4 to Prolog.

##### Reservations people had about this

Meng 

* did not like how it looked like we were piggybacking on another legal DSL (Logical English)

* did not like how the backend took longer than one might like (I cannot remember exactly how long) to handle requests. That said, YM and Joe would note that this is not a foundational issue but rather an engineering-level one --- it's something that can solved with some engineering effort.

YM also thinks, based in part on feedback from other members of the team, that the interfaces (in a software design sense) between the Logical English backend and the frontend / other callers could be improved and made more ergonomic. But this also is something that could probably be achieved with a reasonable amount of effort / time.

One might also have some reservations about how working with, eg., Swi-Prolog and having the client be totally in-browser and adding in more advanced Q&A functionality isn't trivial (though Joe has already worked this out).

Ultimately, though, YM's personal opinion is that there's no reason to hang on to this if we're overhauling things from the ground up.

#### Lessons

* For explainability, it might be enough, at least in the short run, to have some way, perhaps a DSL, for annotating with metadata the things we want to expose to downstream consumers, so that you can control what gets logged or 'explained', and maybe also the phrasing / *how* it gets explained.

	* To put it another way, there is a *prima facie* tension between wanting an encoding that (i) is faithful to the original text / legalistic, more technical concerns and yet (ii) can support explanations that are couched in less legalistic language and more understandable to ordinary users. Metadata facilities might be a good enough way to solve this.

#### Status

Depends on Meng, I guess. But I imagine we'll want to have this at least as a back up option, in case we don't have something else that can do at least as much when a usecase rolls around. 

### The MathLang system and transpiler

See "The 'Explainable' codebase" in the [codebase](./codebase/index.md) file.

### Natural Language Generation

The current codebase for NLG is in [natural4/src/LS/NLP/NLG.hs](https://github.com/smucclaw/dsl/blob/main/lib/haskell/natural4/src/LS/NLP/NLG.hs), and the grammars it is based on are in [natural4/grammars](https://github.com/smucclaw/dsl/tree/main/lib/haskell/natural4/grammars).

The only place where these are/were used is the [web form generation](./index.md#web-form-generation). The module [Purescript.hs](https://github.com/smucclaw/dsl/blob/main/lib/haskell/natural4/src/LS/XPile/Purescript.hs#L39-L47) imports functions from [NLG.hs](https://github.com/smucclaw/dsl/blob/main/lib/haskell/natural4/src/LS/NLP/NLG.hs), and uses them to construct questions out of the Rules.

#### How it work(s|ed)

The cells inside the [Rule](./codebase/rule_ast.md) contain freeform text in different cells. The code in NLG.hs was built based on the two examples of PDPA and Rodents and Vermins, and it made assumptions on which kinds of grammatical constructions appear in which fields. We wrote a handcrafted lexicon that contained exactly the vocabulary needed for the two use cases, and we made sure that the verbs had the right subcategories.

We (Maryam and Inari) also did some smaller experiments in generating the lexicon automatically with UD parser. It was surprisingly good for getting the valencies of the verbs, but there was still a substantial amount of manual checking and correction needed. But given that the next use case didn't use the web app, this system was never needed in practice.

#### Status

This was in use for the PDPA use case and the Rodents and vermin demo, both from 2021/2022. In the insurance use case, we shifted to Logical English, and it didn't use the GF-based NLG at all.


## Visualizations

The Natural L4 ecosystem also allows you to make useful visualizations from a L4 specification.

There is more detailed discussion of the relevant codebases for these sub-systems [in the Codebase section](./codebase/index.md). 

[TODO, high level]


## Natural L4 syntax specification

Finally, [a specification of sorts of the Natural L4 syntax is available here.](https://l4-documentation.readthedocs.io/en/stable/docs/returning-specification.html)

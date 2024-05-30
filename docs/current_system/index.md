# Overview of the current system

As [the current user-facing documentation puts it](https://l4-documentation.readthedocs.io/en/stable/docs/links-returning-users.html),

> while there is one generic L4 syntax, L4 really admits of different fragments, each with their own specialized semantics, corresponding to the various transpilers.

We'll systematically discuss each of these dialects / transpilers and their statuses when we [discuss the codebase in more detail](./codebase/index.md). But before doing that, let's briefly survey what I (YM) take to be the most currently functional components of the L4 ecosystem --- or, to put it another way, what one can currently do with L4.

To clarify, this page aims to introduce the L4 ecosystem by highlighting the the most interesting things that can be done with it, and its key dialects. It does *not* discuss in-the-weeds details about the codebases or their architecture --- see the [Codebase section](./codebase/index.md) for that.

Starting from a Natural4 encoding of a set of legal rules, you can automatically:

  - *parse* the spreadsheet CSV into an **internal representation**: see [codebase/natural4](./codebase/natural4.md)
  - *generate* a **web form "expert system"** that interviews an end-user and returns a result.
	- The [simple version](./webforms.md#propositional-logic-only-decision-support-web-app) of this only deals with propositional logic.
	- The [Logical English version](./logicalenglish.md) deals with numbers and dates too
	- New infrastructure uses the [mathlang codebase](./codebase/generic_mathlang.md).
  - *visualize* the **simple Boolean decision logic**, which is the subject of constitutive rules,
	- as a [black-and-white SVG ladder diagram](./codebase/visualizations.md#simple-ladder-svgs)
	- as an [interactive HTML widget](./codebase/visualizations.md#interactive-ladder-html)
  - *visualize* the **state transition logic**, which is the subject of regulative rules,
	- as a [Petri Net](./codebase/visualizations.md#petri-net-stuff)
  - *statically analyze* the **state transition logic**
	- using [Maude](./codebase/natural4.md#maude)

---

## The MathLang system and transpiler

The MathLang codebase is intended to produce a Javascript runtime suitable for in-browser execution of decision rules originally encoded in L4.

See:

- [The 'Explainable' codebase](./codebase/explainable.md)
- [mathlang](./codebase/mathlang.md)
- [generic mathlang](./codebase/generic_mathlang.md)

---

## Natural Language Generation

The current codebase for NLG is in [natural4/src/LS/NLP/NLG.hs](https://github.com/smucclaw/dsl/blob/main/lib/haskell/natural4/src/LS/NLP/NLG.hs), and the grammars it is based on are in [natural4/grammars](https://github.com/smucclaw/dsl/tree/main/lib/haskell/natural4/grammars).

The NLG codebase was used by the [web form generation](./webforms.md). The main goal was to convert the conditions in the rules into questions.

**For a longer description, see [NLG](./codebase/nlg.md).**

### Status

This was in use for the PDPA use case and the Rodents and vermin demo, both from 2021/2022. In the insurance use case, we shifted to Logical English, and it didn't use the GF-based NLG at all.

We have ambitions to restart the NLG efforts—more in the dedicated page for [NLG](./codebase/nlg.md).

## Natural L4 syntax specification

Finally, [a specification of sorts of the Natural L4 syntax is available here.](https://l4-documentation.readthedocs.io/en/stable/docs/returning-specification.html)

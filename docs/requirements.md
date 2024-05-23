# Requirements, User Personas, Common Use Cases

This document attempts to reflect the overall system requirements.
This is not the first time we have attempted to write this sort of
thing. You you may encounter similar documentation that was written at
different times in the life of the project.

For example, see also
- https://l4-documentation.readthedocs.io/en/stable/docs/quickstart-why-use-L4.html
- https://l4-documentation.readthedocs.io/en/stable/docs/returning-L4-and-law.html

## User Personas

There are three main sets of user personas.

### End-User Trying To Cope With A Legal Situation

We include in the definition of "legal situation" any complex rule-based scenario, including
- dealing with traffic fines
- claiming compensation from an airline
- understanding an insurance policy
- understanding an employment or rental contract

These end-users use web apps and mobile apps that often have the structure of a form-based, interactive expert system.

They also may use a chatbot interface, perhaps partially powered by a ChatGPT type engine.

When we say we want to serve these personas, we mean we want to
provide components of the various tech stacks that are involved in
such apps. These apps may involve a front-end involving interactive
visualizations; a back-end serving a rule engine API; a middle tier
producing human-readable explanations. These are all components which
could be part of the L4 system.

### Legal Drafter Writing a Regulation or a Contract

Legal drafters may be working solo or as part of a team.

They may be working on a contract, or legislation, or regulation.

They may be drafting "from scratch" a new piece of text; or they may be trying to tweak an existing text.

### A Computer

Computers need to consume and process the machine-readable
representations, so as to be able to help the above categories of
human users.

We live in exciting times. We can also use computers to bulk transform
existing natural language rules (laws, regulations, contracts) into L4
for review, approval, and further transformations to support the above
applications.

## Business Context

LLM hallucinations are causing concern. In high-stakes domains,
including legal, applications involving LLMs are motivated to
complement the LLM components with more classically logical, rigorous,
rule-engine-based components which provide better explainability,
source isomorphism to upstream rules, and editability.

The "Rules as Code" movement is slowly gaining steam in advanced economies:
https://www.govcms.gov.au/news-events/news/govcms-announces-enterprise-adoption-rules-code

Conventional web applications that do not have an LLM component can
also benefit from a rule-engine, L4, CNL, DSL, approach 

All of the above provide an opportunity for a legal DSL to answer some
of these demands.

## Specific Use Cases

### State Transition Reductions to Residuals

Given a set of facts and a list of events that have happened in the
past, specialize a set of rules down to only those that are relevant
under the givens.

Deon Digital's CSL reduces a given contract to a residual, after running a bunch of events against it.

### Planning Problems

End-users are often concerned with these questions:

* "what's going on?"
* "what am I supposed to do, and how soon?"
* "if I want to get from here to there, what do I need to do?"

### Natural Language Generation

#### Micro Level

If we have a complex rule written in L4, we want to be able to convert
that rule "losslessly" into natural language, for presentation to an
end-user or to a legal drafter.

#### Macro Level

We want to text plan an entire document to the point it can be signed.







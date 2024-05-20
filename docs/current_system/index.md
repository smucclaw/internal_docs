# Overview of the current system

As [the current user-facing documentation puts it](https://l4-documentation.readthedocs.io/en/stable/docs/links-returning-users.html),

> while there is one generic L4 syntax, L4 really admits of different fragments, each with their own specialized semantics, corresponding to the various transpilers. 

We'll systematically discuss each of these transpilers and their statuses when we [discuss the codebase in more detail](https://l4-documentation.readthedocs.io/en/stable/docs/links-returning-users.html). But before doing that, let's briefly survey what I take to be the most functional components of the current system.

## The (currently-most-functional) transpiler pipelines / components that don't involve visualization

### Web form generation (via the JSON Schema transpiler)

One useful thing you can do with L4 is to scaffold a web form app from an L4 specification. 

[The relevant docs](https://github.com/smucclaw/documentation/blob/main/docs/webform.rst) explain how the web form generation works in some detail. [The JSON schema transpiler docs](https://github.com/smucclaw/documentation/blob/main/docs/transpilers-json-schema.rst) are also relevant.

**Historical Context:** This was motivated by that use case with the insurance company. 

**Status:** Still relevant, 
* though YM thinks that there's quite a bit that could be improved, especially with regards to the interfaces (in the software design sense). 
* And ideally, we'd also want to re-examine the semantics of the schema definition constructs.
* The JSON Schema transpiler also needs some work. [TODO: Add more detail here on what kind of work]


**NOTE:** The [example form app repo](https://github.com/smucclaw/example-l4-form-app) needs some work. Haven't really bothered polishing it because we'll probably want to improve the web app generation system in more fundamental ways.

### The Logical English transpiler

Docs https://github.com/smucclaw/documentation/blob/main/docs/transpilers-logical-english.rst

### The MathLang system and transpiler

TODO

### Natural L4 syntax specification

Finally, [a specification of sorts of the Natural L4 syntax is available here.](https://l4-documentation.readthedocs.io/en/stable/docs/returning-specification.html)

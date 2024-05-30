# The Logical English transpiler #

TODO: Add discussion of what this is and how this is related to the web app stuff

[Docs for this can be found on the user-facing doc site](https://github.com/smucclaw/documentation/blob/main/docs/transpilers-logical-english.rst)

There are some ways in which the code for the transpiler could be cleaned up. But that work has been deferred for now, because it's not clear we want to be using this, going forward.

## Historical Context ##

### Motivation ###

This was motivated by that use case with the insurance company.

If memory serves me right, we went for this because

* we thought that some form of logic programming would make for a good foundation for the system --- eg, it would be simple to extend it with facilities for abductive reasoning. (You could of course also do this with more work in Haskell.) And some of the designers/implementers of this dialect of L4 had a strong interest in logic programming.

* Logical English was especially convenient to build upon because it is a kind of CNL wrapper of Prolog -- it's basically Prolog with a natural-language-y facade. In that way, Logical English made it a lot easier to go from a CNL like Natural L4 to Prolog.

### Reservations people had about this ###

Meng

* did not like how it looked like we were piggybacking on another legal DSL (Logical English)

* did not like how the backend took longer than one might like (I cannot remember exactly how long) to handle requests. That said, YM and Joe would note that this is not a foundational issue but rather an engineering-level one --- it's something that can solved with some engineering effort.

YM also thinks, based in part on feedback from other members of the team, that the interfaces (in a software design sense) between the Logical English backend and the frontend / other callers could be improved and made more ergonomic. But this also is something that could probably be achieved with a reasonable amount of effort / time.

One might also have some reservations about how working with, eg., Swi-Prolog and having the client be totally in-browser and adding in more advanced Q&A functionality isn't trivial (though Joe has already worked this out).

Ultimately, though, YM's personal opinion is that there's no reason to hang on to this if we're overhauling things from the ground up.

## Lessons ##

* For explainability, it might be enough, at least in the short run, to have some way, perhaps a DSL, for annotating with metadata the things we want to expose to downstream consumers, so that you can control what gets logged or 'explained', and maybe also the phrasing / *how* it gets explained.
  * To put it another way, there is a *prima facie* tension between wanting an encoding that (i) is faithful to the original text / legalistic, more technical concerns and yet (ii) can support explanations that are couched in less legalistic language and more understandable to ordinary users. Metadata facilities might be a good enough way to solve this.

## Status ##

Depends on Meng, I guess. But I imagine we'll want to have this at least as a back up option, in case we don't have something else that can do at least as much when a usecase rolls around.

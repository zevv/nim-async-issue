# TL;DR

There are two incompatible async I/O libraries available for Nim, which is not
a good thing given the fundamental layer at which these libraries operate. The
risk is that this might split the library ecosystem. I hope to find a
resolution for this situation that makes everybody happy.

# Background

Over the last few weeks/months I have seen a number of discussions on the Nim
forum and the #nim IRC channel about the state of the Nim async library. After
speaking with some (but not all!) of the people involved I figured it is time
to make this into an official discussion to see how this can be resolved and we
can move forward.

Here is the back story:

- The Nim stdlib comes with a library for doing async (network) I/O, consisting
  of asyncmacro, asyncdispatch, asyncnet and ioselectors. This has been part of
  the standard library for a few years, and is the base layer on which things
  like asyncHttp are built.

- Implementing async I/O is not trivial, and the Nim libraries has had some
  bugs, missing features or other issues (references). Some were fixed and
  addressed in the past, but some are still open (references). Also, some
  issues are more serious then others: some are considered blockers for a 1.0
  Nim release (reference).

- A bit over a year a go a third-party implementation of async I/O called
  `Chronos` (reference) was created by people who had specific demands on the
  I/O layer that the Nim std lib could at time not fill. Chronos was originally
  a fork from nim async library, but has undergone some breaking API changes.
  Chronos has seen steady development over the last year, diverging more and
  more from Nim async, but also fixing some of the issues that were in the Nim
  standard library.

# The problem

Given the above history, the Nim community has now two different but
incompatible Async implementations. Async I/O is an important piece of core
infrastructure which provides the foundation for a lot of other protocol
implementations. Having two incompatible and mutually exclusive async
frameworks is probably not a good thing for the community - shoud I implement
my new protocol on top of the one or the other? Or both? What if I choose the
one but want to use a 3d party lib that depends on the other?

Recently things have slightly "escalated" in various threads on the Nim forum -
users asking questions or noting issues with Nim async are pointed to Chronos
as a solution to their problem. While this answers is perfectly legit and might
help the user with their problem, this might give rise to confusion among new
users, and does not help the current stdlib to evolve and get better.

# Where to go from here?

I think we should get this resolved, and I hope to start a discussion here so
we can figure out the right thing to do. Some questions?

- Do we accept the status quo? There are two available implementations, and
  users should be free to choose?
- Should the Nim stdlib drop the current implementation in favour of chronos?
- Should we put work into getting the two implementations to cooperate?
- Can we add a Chronos-compatibity layer on top of the nim libs, or add a
  nim-compatibility layer on top of Chronos?

Here are the important points that I think should be addressed in this
discussion:

- API stablity: The nim Async layer tries to provide a stable API on which
  other protocols have been implemented. Chronos has had good reasons to make
  changes, but this broke compatibility with existing and future Nim-async
  libraries.

- Code quality: At this time Chronos is seeing a lot of active development
  backed by a commercial party. This simply allows Chronos to fix more of the
  technical debt and add more features. A lot of nim stdlib development is done
  by volunteers which have to do this in their free time for no pay.

- Nim 1.0 is coming up. We probably want to resolve this and have a good story
  before the release.

# Rules for the discussion

- Given the nature of past discussions about this subject, I would like to ask
  all parties involved to refrain from having any of these elsewhere while this
  RFC is on the table. I think noo ne benefits from a forum full of threads
  where the good and the bad of implementations are compared and mud gets
  thrown around.

- I try to see myself as neutral party in this discussion, and I plan to
  moderate this discussion and remove or move off-topic posts to keep the
  discussion focused on the above questions.

## References

References go here.

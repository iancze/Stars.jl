Stars.jl
========

Nothing to see here

Benefits of a Julia implementation of StellarSpectra

No memory management issues of SuiteSparse.

What is the core functionality that we would need to produce?

Basically, a way to write down the model as a function of the many, many parameters that exist. And, a way to reasonably sample it. Coming up with a good sampler to take care of the ~100's of parameters is the tricky part.

However, reimplementing everything else in Julia should be fairly straightforward. Simply designing a model as a function of parameters would be straightforward. But I have a feeling I might still end up in the Gibbs situation.

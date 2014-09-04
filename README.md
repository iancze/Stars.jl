Stars.jl
========

Nothing to see here

Benefits of a Julia implementation of StellarSpectra

No memory management issues of SuiteSparse.

What is the core functionality that we would need to produce?

Basically, a way to write down the model as a function of the many, many parameters that exist. And, a way to reasonably sample it. Coming up with a good sampler to take care of the ~100's of parameters is the tricky part.

However, reimplementing everything else in Julia should be fairly straightforward. Simply designing a model as a function of parameters would be straightforward. But I have a feeling I might still end up in the Gibbs situation.


Ability to easily parallelize results across different spectra. For example, the ~50 different LkCa15 spectra w/ time varying veiling continuum.

There will have to be some caching of model results, and thought about how to properly design the sampler such that certain steps could be properly parallelized.

Since the sparse matrix interface should be easier to interact with, we could also pare all of the region parameters into a single jump per order.

Covariance update could update all regions and global at once, meaning cholfact only needs to happen once per rotation of gibbs. This would need to be a jump in about 30-60 parameters. 

It seems like there could be some very smart speedups for sampling things in parallel.

Parameters Type abstract type. Each variation of parameter combinations is a concrete type. In the guts of the code, we can have different functions that dispatch off of which parameters were actually given (ie, if no alpha, don't interpolate in that dimension), or if no logg, do a different thing.

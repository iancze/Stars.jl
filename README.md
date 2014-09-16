# Stars.jl

Julia implementation of Python code for StellarSpectra. Copyright Ian Czekala 2014.

You just wrote all that Python code..., why redo this in Julia?

## Some Benefits

**No memory management issues of SuiteSparse.** This is a major headache that would probably mostly disappear with Julia.

**Functionality extension** Whenever one might want to add functionality, this is done in a blend of *C*, *cython* and *Python*, making coordination and debugging rather slow and the mental load large.

## Potential stumbling blocks

**Random walk Metropolis (RWM) inside of Gibbs sampler** Need to learn and extend the MCMC package to fit these needs. Sample implementation in `metropolis.jl`. Basically, there will need to be a lot of work to get this working for my needs, similar to how I needed to extent emcee. There are probably some basic ideas I can borrow from, though.

The actual implementation of the sampler is not very difficult. The whole `Task` ability of Julia might work very nicely here. Also, I think a GibbsController type would need to be made. Each of these sample a likelihood function which takes a model, which has a state. Basically, we just have two models walking around the whole time?

## Mostly done

* generation of sparse matrices with covariance kernels
* Doppler shift, FFT, downsampling (using `Grid.jl`)

## Potential types

* 1D spectrum
* 2D sparse covariance matrix
* DataSpectrum (for one order)
* Parameters
* StellarParameters

## Potential functions

    update()
    evaluate()
    downsample()
    revert()

Where is the state of the model stored?

## Parallel implementation

Basic prototype in `parallel.jl`. In this model it would be possible to have a separate process running to do the synthetic photometry. In a masked sense, we could estimate what flux would be missing from the actual data spectrum.

The master process would deal with the stellar parameters. The child processes would have to deal with the RWM specific to these orders.

What is the core functionality that we would need to produce?

Basically, a way to write down the model as a function of the many, many parameters that exist. And, a way to reasonably sample it. Coming up with a good sampler to take care of the ~100's of parameters is the tricky part.

However, reimplementing everything else in Julia should be fairly straightforward. Simply designing a model as a function of parameters would be straightforward. But I have a feeling I might still end up in the Gibbs situation.

Ability to easily parallelize results across different spectra. For example, the ~50 different LkCa15 spectra w/ time varying veiling continuum.

There will have to be some caching of model results, and thought about how to properly design the sampler such that certain steps could be properly parallelized.

Since the sparse matrix interface should be easier to interact with, we could also pare all of the region parameters into a single jump per order.

Covariance update could update all regions and global at once, meaning cholfact only needs to happen once per rotation of gibbs. This would need to be a jump in about 30-60 parameters.

Parameters Type abstract type. Each variation of parameter combinations is a concrete type. In the guts of the code, we can have different functions that dispatch off of which parameters were actually given (ie, if no alpha, don't interpolate in that dimension), or if no logg, do a different thing.

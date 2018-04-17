```@meta
DocTestSetup = quote
    using Stripeline
end
```

# Random number generators

Although Julia already implements a number of pseudo-random number generators,
Stripeline implements its own generators. Strip re-implements the same
generators used in the pipeline of the Planck/LFI instrument, which provided
several types of distributions:

1. Uniform distribution ([`Flat128RNG`](@ref)), with period 2^128;
1. Gaussian distribution ([`GaussRNG`](@ref));
1. $1/f^2$ distribution ([`Oof2RNG`](@ref));
1. $1/f^α$ distribution, with $α < 2$  ([`OofRNG`](@ref)).

Each generator but `Flat128RNG` uses a simpler generator internally. This
generator must sample from a given distribution, but it does not need to be a
generator provided by Stripeline. For instance, the Gaussian generator
`GaussRNG` employs an uniform generator, which can either be `Flat128RNG` or one
of the generators provided by Julia like `MersenneTwister`. For instance, here
is an example which shows how to use `Flat128RNG`:

```@repl rngexample1
using Stripeline # hide
gauss1 = GaussRNG(initflatrng128(1234))
print([randn(gauss1) for i in 1:4])
```

We use `initflatrng128`, as it creates a Flat128RNG object with some sensible
defaults (specifically, it is configured to produce the same sequence of random
numbers as the ones produced by the Planck/HFI pipeline, if the seeds are the
same). And here is the same example, using a `MersenneTwister` generator:

```@repl rngexample1
gauss2 = GaussRNG(MersenneTwister(1234))
print([randn(gauss2) for i in 1:4])
```

Of course, the numbers are different. They are however drawn from the same
distribution (Gaussian curve with mean 0 and σ=1).

## Uniform generator

```@docs
Flat128RNG
initflatrng128
```

## Gaussian generator

```@docs
GaussRNG
```

## $1/f^2$ generator

```@docs
Oof2RNG
```

## $1/f^α$ generator (with $α < 2$)

```@docs
OofRNG
```

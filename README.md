# Traditional CS Methods

## Objectives
This repo provides some of the most used and known compressed sensing reconstruction
models, aswell as some basic input signals that can used in order to see the difference
between a very sparse and periodic signal with less behaved ones. 

## Available Reconstruction Algorithms
### Orthogonal Matching Pursuit (OMG)

This is a greedy algorithm that basically aims to find in every iteration the most 
correlated component. Therefore the number of iterations is the same as the outputted 
sparsity. The two main stopping criteria are sparsity, i.e., same number of iterations
as sparsity; and tolerance, which stops given the error is lower than the tolerance or the 
max number of iterations is reached. 

Fast and reliable. It works well with different types of signals, and serves as a
benchmark for most work in compressed sensing. 

### Compressed Sampling Matching Pursuit (CoSaMP)

Also a greedy algorithm, but the main difference is that it gets 2 * sparsity of components
on each iteration. This makes the algorithm extremely fast, as usually it only needs a few
iterations, but on the downside, it requires sparsity as an input (or at least an estimate),
and also doesn't show such reliability with differnt signals. Overall it's very good with
kwown and behaved signals.

### Stagewise Orthogonal Matching Pursuit (StOMP)

A greedy algorithm that accepts all components that are below a given threshold. This
algorithm can work really well with images, as some literature have already shown, but,
in all honesty, it's yet to prove itself in the applications used here. It requires a threshold
value, which not always is easy to find and sometimes is only acquirable by brute force.
The results are not as consistent as the other two greedy methods, but it's just as fast.

### Basis Pursuit

This is the orignal method designed for CS, as it uses convex minimization in order
to find the componenets. It's definetely the slowest of the bunch and also not as realiable.
If the signal is not very well behaved, it will probably take very long to solve and not
find any useful components. On the other hand, it does provide the (theoretical) most 
accurate result given that the signal is perfectly sparse.

## Input Signals

### Sinusoidal
Sinusoidal signal given a frequency and amplitude.

### Normal
Generates one (or more) gaussian normal curves given a standard deviationa and mean. 
Note: Given that K is large, the signal will approximate to a periodic sinusoidal signal,
and the uniqueness of the Normal curve might be lost.

### Spikes
Generates peaks in a array of zeros, representing a raw sparse signal in the given domain. This
is specially useful as the sparsity, frequency range and amplitude is variable. 

### Saw
A saw tooth signal, in which inclination and shape can be altered. This is useful as the 
discontinuities show a frequency spectrum and forces the reconstruction to set a cutoff.

### Chirper
A sinusoidal signal that increases frequency as time passes by. This is useful to show 
the difficulty of reconstructing completely aperiodic singals. The output when using this
input will almost always be very wrong, as the theory behind CS was made for signals that
have opposite proprieties. 

## How to use

1. Create a base signal object and provide desired parameters. If CoSaMP will be used, 
K will be used and therefore must be given here. 
2. Add signals to the input, i.e., build signals that are composed of different ones.

```
sg = BaseSignal(N, K, maxFreq, minFreq, maxAmp)
x = sg.addSignal('sinusoidal')
```

3. Build base compression object.
4. Sample signal
5. Compress signal

```
cs = CompressedSensing(x, M)

randomMatrix = 'bernoulli'
phi = cs.sampling(randomMatrix)

basis = 'fft'
psi = cs.compression(basis)

y = phi @ x
theta = phi @ psi
```

6. Build Reconstruction object
7. Reconstruct

```
r = Reconstruction(theta, y, x, basis)
s, xrec = r.reconstruct('omp')
```

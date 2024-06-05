# Multifidelity Monte Carlo

A two-level version of MFMC is implemented in `src/two_level_MC.py`. The formulas for the MSE estimation are taken from the paper on MLMC [Krumscheid et al. 2020](https://www.sciencedirect.com/science/article/pii/S0021999120302400) and [Amela et al. 2019](https://zenodo.org/records/3235833), and the implementation is also heavily based on [Peherstorfer et al. 2016](https://doi.org/10.1137/15M1046472).

A paper of MFMC applied to UQ with spent nuclear fuel is under preparation.

## Usage

See `example.ipynb`, or following example:

```

>>> # Generate data or import simulation results:
>>> M = 10**4  # Number of cheap simulations.
>>> N = 20  # Number of expensive simulations.
>>> d = 30  # Input dimension
>>> xs = np.random.randn(M, d) # Example: M input samples of dimension d.
>>> ys = np.array(list(map(hi_fi_model, xs[:N])))  # N simulations with expensive high-fidelity model.
>>> ysSurr = np.array(list(map(low_fi_model, xs)))  # M simulations with cheap low-fidelity model.
>>> print(ys.shape, ysSurr.shape)
(20,) (10000,)
>>>
>>> # Apply MFMC to get moment estimates:
>>> results = get_two_level_estimates(ys, ysSurr[:N], ysSurr, calculate_MSEs = True, adjust_alpha = True)
>>>
>>> # Computed moments:
>>> results['moments MC']  # Mean, variance, skewness, kurtosis
array([ -4.47172337,  24.05847351,  14.71051787, 943.96656693])
>>> results['moments MFMC']
array([-4.47758822e-01,  6.02057815e+01, -2.17309562e+01,  1.05442748e+04])
>>>
>>> # Estimated Mean Square Error of the estimations (not implemented for fourth moment yet):
>>> results['MSEs MC']
array([1.20292368e+00, 2.23019071e+01, 1.53800313e+03, 0.00000000e+00])
>>> results['MSEs MFMC']
array([4.84667872e-02, 4.91110539e+00, 3.80873430e+02, 0.00000000e+00])

```


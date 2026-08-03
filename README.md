# Stellar Luminosity Hands-on

Linear and polynomial regression implemented from scratch (pure numpy, no sklearn) on a small stellar mass vs luminosity dataset.

## Contents

All the technical work lives in [stellar_luminosity_hands_on.ipynb](stellar_luminosity_hands_on.ipynb), already executed with its plots and results:

1. Environment and data loading
2. Exploration (mass vs luminosity)
3. Vectorized functions: `predict`, `compute_cost`, `compute_gradient`, `gradient_descent`
4. Linear model (`X = [mass]`) + convergence diagnostics across several learning rates
5. Polynomial model (`X = [mass, mass^2]`), reusing the same functions
6. Comparison of both models (cost, MSE, residuals)
   - **6.1 Bonus:** log-log transformation (`log(mass)` -> `log(luminosity)`), exploiting that the real mass-luminosity relation is a power law
7. Interpolation (mass=1.3) vs extrapolation (mass=5.0)
8. Reflection on AI / AGI

## Dataset

10 synthetic observations for practice, inspired by the non-linear shape of the main sequence. Not meant to support real astrophysical conclusions, just to practice the algorithm.

| mass (Msun) | luminosity (Lsun) |
|---|---|
| 0.6 | 0.15 |
| 0.8 | 0.35 |
| 1.0 | 1.00 |
| 1.2 | 2.30 |
| 1.4 | 4.10 |
| 1.6 | 7.00 |
| 1.8 | 11.2 |
| 2.0 | 17.5 |
| 2.2 | 25.0 |
| 2.4 | 35.0 |

## Main result

All three models trained with the same `gradient_descent`, compared by MSE in real luminosity units:

| Model | Features | Parameters | MSE | RMSE |
|---|---|---|---|---|
| Linear | `[mass]` | 2 | 19.590 | 4.426 |
| Polynomial | `[mass, mass^2]` | 3 | 0.630 | 0.794 |
| Log-log (bonus) | `[log(mass)]` | 2 | 0.062 | 0.248 |

The straight line fails to capture the curve (large systematic error, even predicts negative luminosities). The polynomial improves a lot by adding `mass^2`. But the log-log model -- which reuses the exact same linear-regression code on transformed data, exploiting that L = a*M^b becomes linear after taking the logarithm -- beats both with fewer parameters than the polynomial, because it uses the correct representation instead of just more capacity. The recovered exponent (b~4.0) lands in the right order of magnitude versus the real astrophysical value (~3.5).

Also, none of the three models should be used to extrapolate far outside the observed range (0.6-2.4 solar masses): at `mass=5.0` all three predictions blow up with no real support in the data (see section 7 of the notebook).

## How to run it

```bash
python -m venv .venv

# windows
.venv\Scripts\activate

# mac/linux
source .venv/bin/activate

pip install -r requirements.txt
jupyter notebook stellar_luminosity_hands_on.ipynb
```

The whole notebook is deterministic (nothing random), so re-running it top to bottom should give exactly the same numbers and plots already saved.

Tested with Python 3.12.1, numpy 2.5.1, matplotlib 3.11.1 (versions are also printed in the notebook's first cell).

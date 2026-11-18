# Crest surface of a transformed Gaussian sea surface: numerical results

This repository contains the code of the numerical section of the paper

> **Kac–Rice formulas for directional extrema of transformed Gaussian random fields, with an application to sea-wave crests**
> (in preparation for *Bernoulli*).

The paper studies a transformed field $W = g(Z)$, where $Z$ is a smooth Gaussian random field and $g$ is a smooth function.
It gives a Kac–Rice formula for the expected size of the set of directional maxima of $W$. For a sea surface in space and time,
this set is the **crest surface** $\{\partial_s W = 0,\ \partial^2_{ss} W < 0\}$, where $s$ is the coordinate along a direction
$u_\theta$. The notebook checks the formula by Monte Carlo simulation on an Airy sea surface with an anisotropic spectrum.

## Authors and collaborators

| Name | Role |
|---|---|
| Mattia Castaldo | PhD student, Laboratoire J.A. Dieudonné (LJAD), Université Côte d'Azur, Nice. Author of the code. |
| Elena Di Bernardino | PhD supervisor |
| Radomyra Shevchenko | PhD advisor |
| Michele Ancona | PhD advisor |

This work is funded by the French National Research Agency (ANR) through the COCHAIR project, within the PEPR Risques (IRiMa) programme.

## Content of the repository

| Path | Content |
|---|---|
| `crest_surface_simulation_airy.ipynb` | Notebook with all the code: model, simulation, estimator, table, figures |
| `data/M_check_h0.2_N40.npz` | Check of the number of modes $M$: 40 fields, $h = 0.2$ |
| `data/sweep_theta0_N2000.npz` | 2000 fields, $\theta = 0$, 7 grid spacings (Table 1, Figures 2–3) |
| `data/direction_aniso_h0.05_N120.npz` | 120 fields, 12 directions, anisotropic field (Figure 4, left) |
| `data/direction_iso_h0.05_N120.npz` | 120 fields, 12 directions, isotropic field (Figure 4, right) |
| `figs/airy_demo.pdf` | Figure 1: the decomposition theorem on one realization |
| `figs/airy_hsweep.pdf` | Figure 2: Monte Carlo mean / Kac–Rice value against the grid spacing $h$ |
| `figs/airy_error.pdf` | Figure 3: meshing error on log-log axes |
| `figs/airy_angle.pdf` | Figure 4: crest area against the direction $\theta$ |
| `table1.tex` | Table 1 in LaTeX |
| `requirements.txt` | Python packages |

The files in `figs/` and `table1.tex` are written by the notebook.

Each `.npz` file contains the array `areas` (crest surface area per unit volume, one value per field, transformation and grid
spacing or direction), the list `names` of the transformations, and the `seeds` of the fields.

## The notebook

The notebook follows the paragraphs of the section *Numerical results* of the paper. Each section starts with the text of the
paper and then runs the corresponding code.

1. **Model construction.** The matrix $A$ of the anisotropic spectrum $\lambda = A\mu$, the covariance matrices
   $\Sigma_{\nabla Z_\theta}$ and $\Sigma_{\nabla\partial_s Z_\theta}$ in closed form, the seven transformations $g$, and the
   Kac–Rice values (with $\mathcal E_3$ by quadrature and the correction $\Delta_\theta(g)$ by Monte Carlo).
2. **Simulation scheme and parameter selection.** The randomized spectral sum $Z_M$ with $M = 2000$ modes, a check of its
   covariance, its excess kurtosis, and the parameters ($Q = [0,8]^2$, $T = 2$, seven grid spacings $h$, $N = 2000$ fields).
3. **Surface estimator.** Marching cubes on $\partial_s W$, with each triangle weighted by the fraction of its area where
   $\partial^2_{ss} W < 0$; the check of the choice of $M$; Figure 1.
4. **Overview of main results.** Table 1: Kac–Rice values and Monte Carlo means at $h = 0.1$ and $h = 0.05$. The parameters
   of the table (transformations, grid spacings, direction, matrix $A$, number of fields) can be changed in one cell.
5. **Reducible and irreducible errors.** Monte Carlo error and meshing error; Figures 2 and 3.
6. **Dependence on the direction $\theta$.** Kac–Rice curves in $\theta$ and Monte Carlo means for the anisotropic and the
   isotropic field; Figure 4.

## How to run

```bash
pip install -r requirements.txt
jupyter notebook crest_surface_simulation_airy.ipynb
```

The first code cell contains the flag

```python
USE_SAVED_DATA = True
```

* `True`: the Monte Carlo runs are loaded from `data/`. The whole notebook runs in about 2 minutes (most of the time is spent on
  the Monte Carlo evaluation of $\Delta_\theta(g)$).
* `False`: all the Monte Carlo runs are simulated from zero and saved in `data/`. On one core this takes about 2 hours for
  Sections 4–5 and about 1.5 hours for Section 6.

If a file of `data/` is missing, the corresponding run is simulated in both cases.

## Reproducibility

Every field is generated from its own seed with `numpy.random.default_rng(seed)`, so the results do not depend on the order of
the computations:

| Run | Seeds |
|---|---|
| fields at $\theta = 0$ (Table 1, Figures 2–3) | 80000, 80001, … |
| fields of the direction study (Figure 4, same seeds in both panels) | 90000, 90001, … |
| check of the number of modes $M$ | 61000, 61001, … |
| covariance check of $Z_M$ | 500000, 500001, … |
| field of Figure 1 | 12 |
| Monte Carlo evaluation of $\Delta_\theta(g)$ | 1 (Table 1), 7 (curves of Figure 4) |

The code was run with Python 3, NumPy 2.5, SciPy 1.18, Matplotlib 3.11 and scikit-image 0.26.

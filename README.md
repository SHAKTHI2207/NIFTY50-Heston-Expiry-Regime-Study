# NIFTY50-Heston-Expiry-Regime-Study

### Evidence from NIFTY 50 Options

**Shakthivel Manivanan**  


This repository contains the computational materials accompanying the study of Heston stochastic-volatility parameter stability across weekly and monthly NIFTY 50 option expiry structures and selected market conditions.
The repository provides the complete computational workflow covering data preparation, implied-volatility estimation, Heston Fourier pricing, calibration, multi-start validation, robustness analysis, parameter stability, and cross-structure validation.


## Overview

This repository contains the computational materials accompanying the research study:

> **Expiry-Structure and Regime Dependence in Heston Model Parameters: Evidence from NIFTY 50 Options**

The study investigates whether the parameterization of the Heston stochastic-volatility model remains stable across different NIFTY 50 option-expiry structures and selected market conditions.

Rather than evaluating Heston only through its ability to fit option prices, the study focuses on **parameter stability, expiry-structure dependence, regime variation, robustness, and cross-structure transferability**.

The central research question is:

> **Does the parameterization of the Heston stochastic-volatility model remain stable across different Indian option-expiry structures and market-regime conditions?**

---

## Research Objectives

The study examines four principal questions:

1. Do Heston parameters differ between weekly and monthly NIFTY 50 options?

2. Does the volatility-of-volatility parameter, $\xi$, vary across different market conditions?

3. Does the spot-volatility correlation parameter, $\rho$, vary across different market conditions?

4. Does a Heston parameterization calibrated to one expiry structure retain its pricing performance when transferred to another expiry structure?

---

## Methodology

The study uses the conventional five-parameter Heston stochastic-volatility framework:

$$
dS_t = rS_tdt + \sqrt{v_t}S_tdW_t^S
$$

and

$$
dv_t =
\kappa(\theta-v_t)dt
+
\xi\sqrt{v_t}dW_t^v.
$$

with

$$
dW_t^S dW_t^v = \rho dt.
$$

The calibrated parameter vector is:

$$
\Theta=(v_0,\kappa,\theta,\xi,\rho).
$$

The parameters represent:

| Parameter | Interpretation |
|---|---|
| $v_0$ | Initial variance |
| $\kappa$ | Speed of mean reversion |
| $\theta$ | Long-run variance |
| $\xi$ | Volatility of volatility |
| $\rho$ | Asset-volatility correlation |

The study uses Fourier-based Heston pricing for calibration and numerical implied-volatility inversion for model-market comparison.

---

## Data and Sample

The empirical analysis uses NIFTY 50 index-option observations from six selected trading dates in 2026.

| Date | Market condition |
|---|---|
| 01-Jan-2026 | Calm |
| 02-Feb-2026 | Budget Event |
| 30-Mar-2026 | Stress |
| 02-Apr-2026 | Stress Persistence |
| 05-Jun-2026 | RBI Event |
| 26-Aug-2026 | Calm |

The final calibration universe contains:

**2,992 eligible option observations**

across:

**6 trading dates**

and

**12 expiry-date calibration slices**

covering weekly and monthly structures.

---

## Sample Construction

The analysis applies a predefined filtering methodology.

### Maturity

Options are retained when:

$$
5 \leq DTE \leq 45.
$$

### Moneyness

Observations satisfy:

$$
\left|\ln(K/S)\right| \leq 0.15.
$$

### Liquidity

Observations satisfy the predefined liquidity criterion based on contracts traded or open interest.

### No-Arbitrage Bounds

Calls satisfy:

$$
\max(S-Ke^{-rT},0) < C < S
$$

and puts satisfy:

$$
\max(Ke^{-rT}-S,0) < P < Ke^{-rT}.
$$

### Market Price

The last traded price is used where available and positive, with closing price used as the fallback observation.

---

## Calibration

The calibration objective is based on vega-weighted squared implied-volatility error:

$$
\mathcal{L}(\Theta)
=
\sum_{i=1}^{N}
w_i
\left(
\sigma_i^{Heston}(\Theta)
-
\sigma_i^{Market}
\right)^2.
$$

The parameter bounds are:

| Parameter | Lower bound | Upper bound |
|---|---:|---:|
| $v_0$ | 0.0001 | 0.25 |
| $\kappa$ | 0.01 | 20 |
| $\theta$ | 0.0001 | 0.25 |
| $\xi$ | 0.01 | 5 |
| $\rho$ | -0.999 | 0.999 |

The calibration workflow uses global and local optimization together with multiple starting configurations.

The Feller condition

$$
2\kappa\theta > \xi^2
$$

is treated as a diagnostic rather than imposed as a calibration constraint.

---

## Numerical Pricing

Heston option prices are calculated using Fourier integration of the model characteristic function.

The pricing representation is:

$$
C(S,K,T)
=
SP_1
-
Ke^{-rT}P_2.
$$

Adaptive numerical integration is used to control Fourier truncation error, particularly for short-dated options.

This is important because short-maturity option pricing can exhibit substantially greater numerical sensitivity to the integration domain than longer-maturity contracts.

---

# Key Empirical Results

## Weekly vs Monthly Calibration

The average calibrated parameters across the six dates are:

| Parameter | Weekly | Monthly |
|---|---:|---:|
| $v_0$ | 0.0365 | 0.0353 |
| $\kappa$ | 15.72 | 6.40 |
| $\theta$ | 0.0236 | 0.0987 |
| $\xi$ | 1.036 | 1.786 |
| $\rho$ | -0.425 | -0.280 |

The results show substantial variation between weekly and monthly parameterizations.

Paired comparisons provide evidence of differences in $\kappa$, $\theta$, and $\xi$ within the six-date sample.

---

## Pricing Performance

Mean implied-volatility RMSE:

| Expiry structure | Mean IV RMSE |
|---|---:|
| Weekly | 0.0606 |
| Monthly | 0.0318 |

The weekly-to-monthly RMSE ratio is approximately:

**1.91x**

This indicates that the weekly calibration slices exhibit larger average in-sample implied-volatility residuals in the study sample.

---

## Parameter Dispersion

Across the twelve calibration slices, the observed parameter ranges are:

| Parameter | Minimum | Maximum |
|---|---:|---:|
| $v_0$ | 0.00736 | 0.12301 |
| $\kappa$ | 1.820 | 19.991 |
| $\theta$ | 0.00874 | 0.23777 |
| $\xi$ | 0.321 | 2.789 |
| $\rho$ | -0.730 | -0.047 |

The dispersion indicates that the estimated Heston parameterization is not constant across the examined expiry-date slices.

---

# Cross-Structure Validation

A central component of the study is cross-structure validation.

The analysis compares:

- Weekly parameters applied to monthly options
- Monthly parameters applied to weekly options

against the corresponding native calibrations.

The mean cross-structure objective penalties are approximately:

| Transfer direction | Mean penalty |
|---|---:|
| Weekly → Monthly | 1.31x |
| Monthly → Weekly | 1.60x |

These results provide direct evidence for examining Heston parameter transferability rather than relying only on native in-sample calibration performance.

---

# Robustness Analysis

The study evaluates robustness by restricting the implied-volatility sample using alternative caps.

Two robustness thresholds are examined:

- IV ≤ 50%
- IV ≤ 100%

The robustness analysis shows that the main calibration patterns persist after restricting extreme implied-volatility observations.

The mean RMSE across robustness slices is approximately:

| IV cap | Mean RMSE |
|---|---:|
| 50% | 0.0382 |
| 100% | 0.0434 |

The April 2026 stress-persistence observation is particularly sensitive to extreme implied-volatility observations and is therefore examined separately in the robustness analysis.

---

# Optimization and Identification Diagnostics

The calibration framework includes several diagnostics designed to distinguish numerical optimization quality from parameter stability.

### Multi-start optimization

Four independent starting configurations were evaluated across the twelve calibration slices.

The calibrated objectives showed an average improvement of approximately:

**80.5%**

relative to the benchmark starting configurations.

### Boundary behaviour

Only one slice exhibited a near-boundary $\kappa$ estimate. No other primary parameter showed systematic boundary proximity.

### Parameter dependence

The calibrated parameters exhibit cross-slice correlations, including:

$$
Corr(\kappa,\theta)\approx -0.654.
$$

This is relevant because parameter variation should not automatically be interpreted as independent movement of each structural parameter.

The study therefore distinguishes:

**parameter instability**

from

**parameter identification.**

---

# Research Interpretation

The results provide descriptive evidence that Heston parameters vary across both expiry structures and selected market conditions.

The findings are consistent with:

- expiry-structure dependence in the calibrated parameterization;
- substantial variation in volatility-of-volatility;
- variation in the spot-volatility correlation parameter;
- higher average weekly calibration error;
- deterioration when parameterizations are transferred across expiry structures.

However, the market-condition groups contain relatively few dates. Consequently, the regime analysis is interpreted as descriptive evidence rather than as a causal estimate of how market stress changes individual Heston parameters.

The study also does not claim that Heston is incapable of pricing NIFTY 50 options. Instead, the focus is on whether **one stable parameterization can be transferred across different expiry structures and market conditions**.

---

# Repository Structure

```text
NIFTY50-Heston-Expiry-Regime-Study/
│
├── README.md
│
├── paper/
│   ├── NIFTY_Heston_Paper.Rmd
│   ├── references.bib
│   └── figures/
│
├── notebooks/
│   └── NIFTY_Heston_V6_Efficient_Calibration.ipynb
│
├── results/
│   ├── calibration_results.csv
│   ├── cross_structure_validation.csv
│   ├── robustness_results.csv
│   └── parameter_stability.csv
│
├── src/
│   ├── heston_pricer.py
│   ├── implied_volatility.py
│   └── calibration.py
│
├── data/
│   └── README.md
│
├── requirements.txt
│
└── LICENSE


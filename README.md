# Filling the Gaps: Benchmarking Time-Series Imputation Methods with PyPOTS

Code and experiments accompanying the Medium article:

**"Filling the Gaps: Benchmarking 9 Time-Series Imputation Methods with PyPOTS"**

This repository benchmarks nine approaches for filling missing values in time-series data using [PyPOTS](https://github.com/WenjieDu/PyPOTS).

We compare simple statistical baselines against deep learning models including recurrent networks, attention-based architectures, diffusion models, and HELIX across two real-world datasets:

* PhysioNet-2012 ICU records
* Beijing Multi-Site Air Quality dataset

The goal is not to declare a universal winner, but to explore how different imputation approaches behave across very different types of time-series data.

## Results Summary

The main observations from the experiments:

* Deep learning approaches generally outperform simple baselines.
* HELIX achieved the best MAE and MRE scores on both datasets in this benchmark.
* SAITS remained highly competitive while requiring considerably less training time.
* CSDI had the highest computational cost and did not perform competitively under the tested configuration.
* Simple baselines can still be useful when speed and simplicity matter.

Results should be interpreted in the context of this setup:

* single random seed
* MCAR point masking only
* no hyperparameter optimization
* limited training budget

## Models Compared

### Baselines

| Model  | Description                      |
| ------ | -------------------------------- |
| Mean   | Feature-wise mean imputation     |
| Median | Feature-wise median imputation   |
| LOCF   | Last Observation Carried Forward |

### Neural Models

| Model       | Description                                                                     |
| ----------- | ------------------------------------------------------------------------------- |
| M-RNN       | Bidirectional recurrent imputation model                                        |
| BRITS       | RNN with learned temporal decay                                                 |
| Transformer | Self-attention based imputation                                                 |
| SAITS       | Self-attention model designed for time-series imputation                        |
| CSDI        | Conditional diffusion model                                                     |
| HELIX       | Hybrid encoding architecture with feature identity and cross-dimensional fusion |

## Datasets

### PhysioNet-2012

A clinical time-series dataset containing ICU patient measurements.

This dataset is commonly used for time-series imputation research because missing values often arise from real clinical workflows.

### Beijing Multi-Site Air Quality

A sensor dataset containing air quality measurements collected across multiple monitoring stations.

Compared with PhysioNet, this dataset has different temporal characteristics and missing-value behavior, making it useful for testing generalization.

## Running the Experiments

This project uses `uv` for dependency management.

Install dependencies:

```bash
uv sync
```

Activate the environment:

```bash
source .venv/bin/activate
```

Run the notebooks:

```bash
uv run jupyter notebook
```

The notebooks cover:

1. Dataset preparation
2. Missing-value generation with PyGrinder
3. Model training
4. Evaluation
5. Result visualization


## Experimental Details

Missing values:

* Natural missingness was preserved.
* Additional MCAR point missingness was injected at 30%.
* Metrics were calculated only on artificially masked values.

Metrics:

* MAE
* MSE
* MRE

Training:

* 50 epochs
* Early stopping patience: 5
* No hyperparameter search performed

## Limitations

This benchmark has several limitations:

* Experiments were run with a single seed.
* Only MCAR point missingness was evaluated.
* MAR/MNAR scenarios were not tested.
* Hyperparameters were not tuned.
* Results may change with longer training or larger datasets.

## References

* PyPOTS: https://github.com/WenjieDu/PyPOTS
* SAITS: Self-Attention-based Imputation for Time Series
* CSDI: Conditional Score-based Diffusion Models for Probabilistic Time Series Imputation
* HELIX: Hybrid Encoding with Learnable Identity and Cross-dimensional Synthesis for Time Series Imputation

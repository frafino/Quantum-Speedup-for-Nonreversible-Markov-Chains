# Circuit implementation for speeding both reversible and non reversible Markov chain's mixing time.

An EPFL project investigating the recent mathematical methods to speedup the mixing time of a non reversible Markov chain.

## Overview
We provide a fully customizable and flexible implementation of the methods proposed by Claudon et al. [^1]. Specifically, this repository implements the first of the two proposed methods, which utilizes access to the time-reversal kernel $P^*$.

Because this framework can also accelerate the mixing time of *reversible* Markov chains, we have extended our implementation to support those as well.

Additionally, we provide a comprehensive project report. This document features a restructured, thorough exposition of the paper's core theoretical framework alongside an in-depth analysis of our experimental results.

## Notebooks

## Requirements

This project requires **Python 3.10+** and relies on the following core scientific and quantum computing libraries:

* **[Qiskit](https://github.com/Qiskit/qiskit) & Qiskit Aer:** For quantum circuit construction, state preparation, and noisy hardware simulation.
* **SciPy:** Used for classical optimization, signal convolution, and linear algebra routines (such as null space calculations).
* **NumPy & Matplotlib:** For numerical matrix operations (e.g., Chebyshev polynomials) and plotting experimental data.
* **IPython:** For notebook-based output rendering.

After having cloned the repository, you can install them via:

```bash
pip install -r requirements.txt
```

[^1]: Claudon, B., Piquemal, J. P., & Monmarché, P. (2025). *Quantum Speedup for Nonreversible Markov Chains*. arXiv preprint arXiv:2501.05868. [Read on arXiv](https://arxiv.org/abs/2501.05868)

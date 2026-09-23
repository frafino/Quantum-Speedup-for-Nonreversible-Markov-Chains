# Quantum Sampling from Markov Chain Stationary Distributions

An EPFL semester project implementing and investigating the quantum algorithm proposed by Claudon, Piquemal & Monmarché (Nature Communications, 2025) for accelerating sampling from the stationary distribution of both reversible and non-reversible Markov chains.

## Overview

We provide a fully self-contained implementation of the first method proposed in [^1], which constructs an approximate reflection through the stationary distribution using access to the time-reversal kernel $P^\star$. The core steps are:

1. **Curved discriminant** — encode the chain's spectral structure via $D = \sqrt{PP^\star}$.
2. **Chebyshev fast-forwarding polynomial** — isolate the leading singular vector $|\pi\rangle$ while suppressing all others within error $\varepsilon$.
3. **GQSVT / GQET circuit** — implement the polynomial transformation on a quantum computer via Generalized Quantum Signal Processing (GQSP).

Because the same framework also accelerates *reversible* chains (via the simpler GQET path, without a Hermitisation ancilla), we provide a dedicated notebook for that case as well.

A full project report is included in `report/`. It contains a restructured exposition of the paper's theoretical framework and a detailed analysis of our experimental results.

## Notebooks

### `01_lazy_birthdeath_chain.ipynb` — Reversible chain (GQET)

Implements the quantum algorithm on a 4-state lazy birth-death chain. Since the chain satisfies detailed balance, the flat discriminant $D$ is symmetric and admits a direct eigenvalue transformation (GQET), without the need for Hermitisation.

Covers:
- Markov kernel definition and detailed-balance verification
- Discriminant, spectral gap, and SPUE construction
- Chebyshev polynomial filter and classical verification ($\nu(D) \approx |\pi\rangle\langle\pi|$)
- Full GQET quantum circuit with GQSP angle computation
- Noiseless simulation and TVD measurement
- Classical vs. quantum complexity comparison
- Controlled depolarising noise analysis

### `02_non_reversible_chain.ipynb` — Non-reversible chain (GQSVT)

Implements the full algorithm on an 8-state non-reversible chain with a deliberately small pseudo-spectral gap ($\gamma_\infty(P) \approx 10^{-3}$), making the quantum speedup clearly visible.

The non-reversible case requires two additional ingredients: **Hermitisation** (an extra ancilla qubit to handle the asymmetric curved discriminant) and a stricter post-selection condition.

Covers:
- Non-reversible kernel with near-reducible block structure
- Time-reversal kernel $P^\star$ and curved discriminant $D = \sqrt{PP^\star}$
- Reversibilisation time $\tau_{\text{rev}}$ search
- Hermitised walk operator and GQSVT circuit
- Post-selection (`s=0`, `h=1`, `y=0…0`) and success rate analysis
- Classical vs. quantum TVD sweep over degree and $\varepsilon$
- Controlled depolarising noise analysis

## Requirements

This project requires **Python 3.10+** and the following libraries:

| Library | Purpose |
|---|---|
| `qiskit` + `qiskit-aer` | Circuit construction, simulation, noisy backends |
| `scipy` | L-BFGS-B optimisation, convolution, null space, `block_diag` |
| `numpy` | Matrix operations, Chebyshev polynomial evaluation |
| `matplotlib` | Plotting TVD curves, histograms, spectral filtering |
| `ipython` | Notebook output rendering (`clear_output`) |

Install all dependencies after cloning:

```bash
git clone https://github.com/<your-username>/<your-repo>.git
cd <your-repo>
pip install -r requirements.txt
```

## Results

On the noiseless Aer simulator:

| Chain | Method | Polynomial degree $d$ | Quantum TVD | Classical TVD (same $d$) |
|---|---|---|---|---|
| Lazy birth-death (reversible) | GQET | 4 | ≈ 0.002 | ≈ 0.167 |
| Non-reversible 8-state | GQSVT | 170 | ≈ 0.10 | ≈ 0.185 |

Eventhough the results for the non reversible chain are not promising, we see that a lower TVD is reached at way lower degrees (look at the notebook).

Both circuits are sensitive to noise: depolarising error rates above $p \approx 10^{-3}$ push the TVD above the $\varepsilon$ threshold, highlighting the need for fault-tolerant hardware.

## References

[^1]: Claudon, B., Piquemal, J.-P., & Monmarché, P. (2025). Quantum speedup for nonreversible Markov chains. *Nature Communications*, 16(1). [https://doi.org/10.1038/s41467-025-56171-0](https://doi.org/10.48550/arXiv.2501.05868)

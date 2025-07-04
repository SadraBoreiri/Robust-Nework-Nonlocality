# Robust Network Nonlocality

[![arXiv](https://img.shields.io/badge/arXiv-2311.02182-b31b1b.svg)](https://arxiv.org/abs/2311.02182)

This repository contains the Python code to reproduce the results and explore the concepts presented in the paper: **"Noise-robust proofs of quantum network nonlocality"**.

## 📝 Overview

Quantum networks, where multiple parties share entanglement from independent sources, open avenues for novel forms of quantum nonlocality. By skillfully combining entangled states and entangled measurements, it's possible to generate strong nonlocal correlations that span the entire network. However, most theoretical demonstrations of these effects have been confined to idealized, noiseless scenarios, assuming perfect pure entangled states and flawless projective measurements.

This work confronts this challenge by presenting **noise-robust proofs of network quantum nonlocality**. We focus on a specific class of quantum distributions generated on the **triangle network** topology, which inherently relies on entangled states and entangled measurements. A cornerstone of our approach is the development of **approximate rigidity** results for local distributions that exhibit properties like "parity token counting" with high probability.

Tackling noise-robust nonlocality without inputs in network scenarios has long been a challenging problem. Previously, the only known results for such quantum distributions on the triangle network relied on the numerical inflation technique, which yielded extremely low noise robustness—around $10^{-7}$ with respect to white noise. In contrast, our result achieves a white noise robustness of approximately 0.7%, representing an improvement of several orders of magnitude. While this level is still beyond current experimental capabilities, it marks a significant theoretical advance and a crucial step toward practical demonstrations of noise-robust quantum nonlocality.

By analyzing quantum distributions that arise from imperfect sources and measurements, we quantify the robustness of network nonlocality to more realistic imperfections. In particular, we establish thresholds for various types of noise, including up to an astonishing ~80% robustness against dephasing noise and ~0.7% for white noise. Moreover, we show that nonlocality persists even for distributions close (in total variation distance) to ideal quantum distributions. These findings open the door to experimentally accessible tests of quantum nonlocality in complex networked systems.

The Python code provided in this repository allows for the numerical simulation and verification of these findings, particularly for the triangle network under various noise models.

---

## 🔬 Theoretical Background & Notebook Details

for each of the sources in the triangle network we consider the quantum state  of the form:

$$
|\psi\rangle = \lambda_0|01\rangle + \lambda_1|10\rangle
$$

This state is distributed by sources within the network (e.g., the triangle network involving three parties, Alice, Bob, and Charlie, each receiving subsystems from two independent sources) and the parties perform a measurment in the following basis:


$$ |\phi_{1,0}\rangle = u |0,1\rangle + v|1,0\rangle $$
$$ |\phi_{1,1}\rangle = v |0,1\rangle - u|1,0\rangle $$
$$ |\phi_{0,0}\rangle = w |0,0\rangle + z|1,1\rangle $$
$$ |\phi_{0,1}\rangle = z |0,0\rangle - w|1,1\rangle $$

### Key Notebooks and Formulations:

The Jupyter notebooks implement the mathematical framework to analyze these networks, primarily using Linear Programming (LP) solved with MOSEK to distinguish quantum nonlocal correlations from classical ones.

1.  **`Noisless_MOSEK.ipynb`**:
    * **Focus**: Establishes the baseline by generating the quantum probability distribution in the *absence* of noise and formulates the LP 
        
    * **LP Formulation**: Sets up the constraints for a local hidden variable model in the network, including probability normalization and independence conditions reflecting the network structure. The objective function then tests if the quantum distribution lies outside the classical polytope.

2.  **`White_noise_LP.ipynb`**: 
    * **Focus**: Investigates the impact of **white noise** on the nonlocality of the network. White noise can be introduced at the source level (affecting the shared state) and/or at the measurement stage. Moreover, the file `LP_WhiteNoise_BEST_Q64.ipynb` explores the improved noise robustness achieved by this method, using more computationally intensive evaluations.
    * **State under Source White Noise**: The initial state $\rho_S = |\psi\rangle\langle\psi|$ is mixed with maximally mixed noise $\rho = (1-W) |\psi\rangle\langle\psi| + \frac{W}{4} I$ for the noise parameter $W$
    * **Noisy Measurements**: Measurements $M_{x_1,x_2}$ can also be affected $M_{x_1,x_2}' = (1-\eta)M_{x_1,x_2} + \eta \frac{I}{4}$ for the noise parameter $\eta$
    * The notebook aims to find the critical noise parameters ($W, \eta$) below which we can guarantee nonlocality. 
3.  **`Dephasing_noise_LP.ipynb`**:
    * **Focus**: Analyzes the robustness of network nonlocality against **dephasing noise**. Dephasing affects the off-diagonal elements of the density matrix in a specific basis.
    * **State under Dephasing Noise**: For the state $|\psi\rangle = \lambda_0 |01\rangle + \lambda_1 |10\rangle$, the dephased state becomes:
        $$\rho_D = (1-d) |\psi\rangle\langle\psi| + d (\lambda_0^2 |01\rangle\langle01| + \lambda_1^2 |10\rangle\langle10|)$$
        The notebook then uses LP to find the critical dephasing parameter $d$.

4.  **`Double_WhiteNoise_plot.ipynb`**:
Analyzes and visualizes the regions of white noise affecting both the sources and measurements, identifying the parameter regimes where nonlocality can still be detected using our method.


5.  **`TV_Noise_LP_MOSEK.ipynb`**:
    * **Focus**: This notebook appears to analyze robustness against a general mixing noise, often parameterized by a visibility $V$, which can be related to Total Variation (TV) distance from an ideal distribution or mixing with white noise.
    * **State under General Mixing Noise**: The noisy state is typically of the form:
        $$
        \rho = V \rho_{\text{ideal}} + (1-V) \rho_{\text{noise}}
        $$
        where $\rho_{\text{noise}}$ is often the maximally mixed state. The LP is used to determine the minimum visibility $V$ for which nonlocality can still be observed.

---

## 💻 Code Structure & Usage

The primary code is organized into Jupyter Notebooks. Each notebook contains:
* Python code for numerical simulations.
* LaTeX annotations explaining the theoretical steps and formulas.
* Implementations of network topologies, quantum states, measurements, and noise models.
* Linear Programming (LP) formulations solved using MOSEK to certify nonlocality.
* Plotting routines to visualize the results (e.g., regions of nonlocality vs. noise parameters).

### Prerequisites:

Ensure you have the following Python libraries installed:
* **NumPy**: For numerical computations.
    ```bash
    pip install numpy
    ```
* **SciPy**: For scientific computing, potentially including optimization routines.
    ```bash
    pip install scipy
    ```
* **MOSEK**: A high-performance solver for large-scale optimization problems, crucial for the LPs. A MOSEK license (free academic licenses are available) and its Python bindings are required.
    ```bash
    pip install mosek
    ```
* **Matplotlib**: For generating plots.
    ```bash
    pip install matplotlib
    ```
* **SymPy**: For symbolic mathematics (used in `Dephasing_noise_LP.ipynb`).
    ```bash
    pip install sympy
    ```
* **tqdm**: For displaying progress bars during lengthy computations.
    ```bash
    pip install tqdm
    ```
* **(CVXPY)**: While MOSEK is used directly, CVXPY might be a useful higher-level interface for some users if they wish to modify the LPs.
    ```bash
    # pip install cvxpy
    ```

### Running the Code:

1.  **Clone the repository:**
    ```bash
    git clone [https://github.com/SadraBoreiri/Robust-Nework-Nonlocality.git](https://github.com/SadraBoreiri/Robust-Nework-Nonlocality.git)
    cd Robust-Nework-Nonlocality
    ```
2.  **Install dependencies:**
    Ensure all prerequisites listed above are installed and MOSEK is correctly configured with a valid license.
3.  **Launch Jupyter Notebook:**
    ```bash
    jupyter notebook
    ```
    Navigate to and open the desired `.ipynb` file (e.g., `LP_WhiteNoise_BEST_Q64.ipynb`). Run the cells sequentially. The embedded LaTeX and comments will guide you through the logic and calculations.

---

## 📊 Expected Outputs

By running the Jupyter notebooks, you should be able to:
* Reproduce the key numerical results and figures presented in the paper, particularly the plots showing nonlocal regions as a function of noise parameters and state/measurement choices.
* Calculate critical noise thresholds for observing nonlocality in the triangle network under different noise models (white noise, dephasing noise).
* Gain a practical understanding of how Linear Programming can be used to certify network nonlocality and assess its robustness.

---
## 📄 Citation

If you use this code or the findings from the paper in your research, please cite the paper:

S. Boreiri, B. Ulu, N. Brunner, and P. Sekatski. *Noise-robust proofs of quantum network nonlocality*. arXiv:2311.02182 [quant-ph] (2023).

```bibtex
@article{Boreiri2023robust,
  title={Noise-robust proofs of quantum network nonlocality},
  author={Boreiri, Sadra and Ulu, Bora and Brunner, Nicolas and Sekatski, Pavel},
  journal={arXiv preprint arXiv:2311.02182},
  year={2023},
  eprint={2311.02182},
  archivePrefix={arXiv},
  primaryClass={quant-ph}
}

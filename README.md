# `README.md`

# Heuristic-Augmented Financial Risk Index (HAFRI): Assessing Financial Statement Risks among MCDM Techniques

<!-- PROJECT SHIELDS -->
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![Python Version](https://img.shields.io/badge/python-3.9%2B-blue.svg)](https://www.python.org/)
[![arXiv](https://img.shields.io/badge/arXiv-2512.04035v1-b31b1b.svg)](https://arxiv.org/abs/2512.04035v1)
[![Journal](https://img.shields.io/badge/Journal-Economics%20(econ.TH)-003366)](https://arxiv.org/abs/2512.04035v1)
[![Year](https://img.shields.io/badge/Year-2025-purple)](https://github.com/chirindaopensource/assessing_financial_statement_risks)
[![Discipline](https://img.shields.io/badge/Discipline-Financial%20Risk%20Management%20%7C%20MCDM-00529B)](https://github.com/chirindaopensource/assessing_financial_statement_risks)
[![Data Sources](https://img.shields.io/badge/Data-Damascus%20Securities%20Exchange-lightgrey)](http://www.dse.sy/)
[![Data Sources](https://img.shields.io/badge/Data-Bloomberg%20Terminal%20(Fundamentals)-lightgrey)](https://www.bloomberg.com/professional/solution/bloomberg-terminal/)
[![Data Sources](https://img.shields.io/badge/Data-Company%20Annual%20Reports-lightgrey)](https://github.com/chirindaopensource/assessing_financial_statement_risks)
[![Core Method](https://img.shields.io/badge/Method-Analytic%20Hierarchy%20Process%20(AHP)-orange)](https://github.com/chirindaopensource/assessing_financial_statement_risks)
[![Analysis](https://img.shields.io/badge/Analysis-Simple%20Additive%20Weighting%20(SAW)-red)](https://github.com/chirindaopensource/assessing_financial_statement_risks)
[![Code style: black](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/psf/black)
[![Type Checking: mypy](https://img.shields.io/badge/type%20checking-mypy-blue)](http://mypy-lang.org/)
[![NumPy](https://img.shields.io/badge/numpy-%23013243.svg?style=flat&logo=numpy&logoColor=white)](https://numpy.org/)
[![Pandas](https://img.shields.io/badge/pandas-%23150458.svg?style=flat&logo=pandas&logoColor=white)](https://pandas.pydata.org/)
[![SciPy](https://img.shields.io/badge/SciPy-%230C55A5.svg?style=flat&logo=scipy&logoColor=white)](https://scipy.org/)
[![PyYAML](https://img.shields.io/badge/PyYAML-gray?logo=yaml&logoColor=white)](https://pyyaml.org/)
[![Jupyter](https://img.shields.io/badge/Jupyter-%23F37626.svg?style=flat&logo=Jupyter&logoColor=white)](https://jupyter.org/)

**Repository:** `https://github.com/chirindaopensource/assessing_financial_statement_risks`

**Owner:** 2025 Craig Chirinda (Open Source Projects)

This repository contains an **independent**, professional-grade Python implementation of the research methodology from the 2025 paper entitled **"Assessing Financial Statement Risks among MCDM Techniques"** by:

*   Marwa Abdullah
*   Revzon Oksana Anatolyevna
*   Duaa Abdullah

The project provides a complete, end-to-end computational framework for replicating the paper's findings. It delivers a modular, auditable, and extensible pipeline that executes the entire research workflow: from rigorous financial statement data validation and expert survey processing to hierarchical weight derivation via AHP, risk scoring via SAW, and comprehensive robustness analysis.

## Table of Contents

- [Introduction](#introduction)
- [Theoretical Background](#theoretical-background)
- [Features](#features)
- [Methodology Implemented](#methodology-implemented)
- [Core Components (Notebook Structure)](#core-components-notebook-structure)
- [Key Callable: `execute_hafri_master_pipeline`](#key-callable-execute_hafri_master_pipeline)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Input Data Structure](#input-data-structure)
- [Usage](#usage)
- [Output Structure](#output-structure)
- [Project Structure](#project-structure)
- [Customization](#customization)
- [Contributing](#contributing)
- [Recommended Extensions](#recommended-extensions)
- [License](#license)
- [Citation](#citation)
- [Acknowledgments](#acknowledgments)

## Introduction

This project provides a Python implementation of the analytical framework presented in Abdullah et al. (2025). The core of this repository is the iPython Notebook `assessing_financial_statement_risks_draft.ipynb`, which contains a comprehensive suite of functions to replicate the paper's findings. The pipeline is designed to be a generalizable toolkit for constructing a **Heuristic-Augmented Financial Risk Index (HAFRI)**, enabling the ranking of fiscal periods by their aggregate exposure to financial risk.

The paper addresses the challenge of quantifying latent financial risk by integrating expert heuristics with objective financial ratios. This codebase operationalizes the paper's framework, allowing users to:
-   Rigorously validate and manage the entire experimental configuration via a single `study_configuration.yaml` file.
-   Process raw expert pairwise comparison surveys to derive consensus weights for risk criteria.
-   Implement the **Analytic Hierarchy Process (AHP)** to decompose risk into Capital Structure, Liquidity, Income, and Cash Flow domains.
-   Apply **Simple Additive Weighting (SAW)** to aggregate normalized financial ratios into a composite risk score.
-   Validate findings against the original study's results for Al-Ahliah Vegetable Oil Company (2008–2017).
-   Conduct automated robustness checks to test the sensitivity of risk rankings to methodological assumptions.
-   Automatically generate a comprehensive technical report and reproducibility package.

## Theoretical Background

The implemented methods are grounded in Multi-Criteria Decision Making (MCDM) theory and financial statement analysis.

**1. Analytic Hierarchy Process (AHP):**
AHP is used to elicit and synthesize expert judgments into a coherent set of priority weights. The problem is decomposed into a hierarchy:
-   **Goal:** Aggregate Financial Risk.
-   **Main Criteria:** Capital Structure Risk (CSR), Liquidity Risk (LR), Income Risk (IR), Cash Flow Risk (CFR).
-   **Sub-Criteria:** 34 financial ratios derived from financial statements.

Weights are derived from pairwise comparison matrices $A$ where $A_{ij}$ represents the relative importance of criterion $i$ over $j$. The principal eigenvector $w$ is approximated using the Row Geometric Mean method, and consistency is validated using the Consistency Ratio ($CR < 0.10$).

**2. Simple Additive Weighting (SAW):**
SAW aggregates the performance of alternatives (fiscal years) across multiple criteria.
-   **Normalization:** Raw ratio values $x_{ij}$ are transformed into commensurable utility scores $r_{ij} \in [0, 1]$ using Min-Max normalization.
    -   **Benefit Criteria (Max):** Higher values reduce risk (e.g., Cash Readiness).
        $$ r_{ij} = \frac{x_j^+ - x_{ij}}{x_j^+ - x_j^-} $$
    -   **Cost Criteria (Min):** Higher values increase risk (e.g., Debt/Equity).
        $$ r_{ij} = \frac{x_{ij} - x_j^-}{x_j^+ - x_j^-} $$
-   **Aggregation:** The composite risk score $V_i$ is the weighted sum of normalized scores:
    $$ V_i = \sum_{j=1}^{n} w_j r_{ij} $$

Below is a diagram which summarizes the proposed approach:

<div align="center">
  <img src="https://github.com/chirindaopensource/assessing_financial_statement_risks/blob/main/assessing_financial_statement_risks_ipo.png" alt="HAFRI Process Summary" width="100%">
</div>

## Features

The provided iPython Notebook (`assessing_financial_statement_risks_draft.ipynb`) implements the full research pipeline, including:

-   **Modular, Multi-Task Architecture:** The entire pipeline is broken down into 28 distinct, modular tasks, each with its own orchestrator function.
-   **Configuration-Driven Design:** All study parameters are managed in an external `study_configuration.yaml` file.
-   **Rigorous Data Validation:** A multi-stage validation process checks the schema, content integrity, and temporal consistency of expert surveys and financial statements.
-   **Advanced MCDM Implementation:** Integrates AHP consistency checking, hierarchical weight composition, and SAW normalization with explicit directionality handling.
-   **Robustness Verification:** Includes automated sensitivity analysis scenarios (e.g., Strict vs. Relaxed Consistency thresholds) to validate the stability of risk rankings.
-   **Reproducible Artifacts:** Generates structured dictionaries and serialized files (CSV, JSON) for every intermediate result, ensuring full auditability.

## Methodology Implemented

The core analytical steps directly implement the methodology from the paper:

1.  **Validation & Preprocessing (Tasks 1-7):** Ingests raw data, validates schemas, enforces accounting identities (e.g., Assets = Liabilities + Equity), and handles missing values/zero denominators.
2.  **AHP Analysis (Tasks 8-15):** Constructs the risk hierarchy, builds pairwise comparison matrices, computes local weights, filters inconsistent experts ($CR \ge 0.10$), and aggregates global weights.
3.  **Ratio Computation (Tasks 16-18):** Defines computational logic for 34 ratios, computes the raw decision matrix $X$, and validates it for outliers and zero variance.
4.  **SAW Analysis (Tasks 19-23):** Configures criterion directionality (Benefit/Cost), normalizes the decision matrix to risk scores $R$, applies global weights to get $V$, and computes composite risk scores $V_t$.
5.  **Ranking & Validation (Tasks 24-27):** Ranks fiscal years by aggregate risk, identifies extreme years, and cross-checks results against the published study values.
6.  **Packaging (Task 28):** Generates a technical report and serializes all outputs into a reproducibility package.

## Core Components (Notebook Structure)

The `assessing_financial_statement_risks_draft.ipynb` notebook is structured as a logical pipeline with modular orchestrator functions for each of the 28 major tasks. All functions are self-contained, fully documented with type hints and docstrings, and designed for professional-grade execution.

## Key Callable: `execute_hafri_master_pipeline`

The project is designed around a single, top-level user-facing interface function:

-   **`execute_hafri_master_pipeline`:** This master orchestrator function, located in the final section of the notebook, runs the entire automated research pipeline from end-to-end. A single call to this function reproduces the entire computational portion of the project, managing data flow between all 28 sub-tasks, including robustness checks and report generation.

## Prerequisites

-   Python 3.9+
-   Core dependencies: `pandas`, `numpy`, `pyyaml`, `scipy`.

## Installation

1.  **Clone the repository:**
    ```sh
    git clone https://github.com/chirindaopensource/assessing_financial_statement_risks.git
    cd assessing_financial_statement_risks
    ```

2.  **Create and activate a virtual environment (recommended):**
    ```sh
    python -m venv venv
    source venv/bin/activate  # On Windows, use `venv\Scripts\activate`
    ```

3.  **Install Python dependencies:**
    ```sh
    pip install pandas numpy pyyaml scipy
    ```

## Input Data Structure

The pipeline requires two primary DataFrames:
1.  **`raw_expert_survey_df`**: A log of pairwise comparisons with columns: `expert_id`, `hierarchy_level`, `criterion_i`, `criterion_j`, `saaty_scale_value`, `comparison_type`.
2.  **`raw_financial_statement_df`**: Audited financial line items with columns: `fiscal_year`, `total_assets`, `current_assets`, `net_operating_cash_flow`, etc. (27 fields total).

## Usage

The `assessing_financial_statement_risks_draft.ipynb` notebook provides a complete, step-by-step guide. The primary workflow is to execute the final cell of the notebook, which demonstrates how to use the top-level `execute_hafri_master_pipeline` orchestrator:

```python
# Final cell of the notebook

# This block serves as the main entry point for the entire project.
if __name__ == '__main__':
    # 1. Load the master configuration from the YAML file.
    with open('study_configuration.yaml', 'r') as f:
        study_config = yaml.safe_load(f)
    
    # 2. Load raw datasets (Example using synthetic generator provided in the notebook)
    # In production, load from CSV/Parquet: pd.read_csv(...)
    raw_expert_survey_df = ... 
    raw_financial_statement_df = ...
    
    # 3. Execute the entire replication study.
    results = execute_hafri_master_pipeline(
        raw_expert_survey_df=raw_expert_survey_df,
        raw_financial_statement_df=raw_financial_statement_df,
        study_configuration=study_config
    )
    
    # 4. Access results
    print(f"Most Risky Year: {results['baseline_results']['ranking'].index[0]}")
```

## Output Structure

The pipeline returns a master dictionary containing all analytical artifacts:
-   **`baseline_results`**: Contains `global_weights`, `decision_matrix`, `normalized_matrix`, `weighted_matrix`, `composite_scores`, `ranking`, and `comparison`.
-   **`robustness_results`**: Contains `scenario_details` and `stability_summary` (rank statistics across scenarios).
-   **`validation_results`**: Contains `weight_comparison`, `score_comparison`, and `summary_report` (discrepancy analysis).
-   **`final_package`**: Contains serialized strings for `technical_report.md`, `README.md`, and CSVs of all matrices.

## Project Structure

```
assessing_financial_statement_risks/
│
├── assessing_financial_statement_risks_draft.ipynb  # Main implementation notebook
├── study_configuration.yaml                         # Master configuration file
├── requirements.txt                                 # Python package dependencies
│
├── LICENSE                                          # MIT Project License File
└── README.md                                        # This file
```

## Customization

The pipeline is highly customizable via the `study_configuration.yaml` file. Users can modify study parameters such as:
-   **Time Horizon:** `start_year`, `end_year`.
-   **AHP Settings:** `consistency_threshold`, `saaty_scale_mapping`.
-   **Ratio Definitions:** Numerator/denominator logic in `feature_engineering_logic`.
-   **SAW Settings:** `criteria_directionality` (Benefit/Cost assignment).

## Contributing

Contributions are welcome. Please fork the repository, create a feature branch, and submit a pull request with a clear description of your changes. Adherence to PEP 8, type hinting, and comprehensive docstrings is required.

## Recommended Extensions

Future extensions could include:
-   **Fuzzy AHP:** Incorporating fuzzy logic to handle uncertainty in expert judgments.
-   **TOPSIS Integration:** Adding Technique for Order of Preference by Similarity to Ideal Solution as an alternative ranking method.
-   **Dynamic Weighting:** Allowing weights to evolve over time based on market conditions.

## License

This project is licensed under the MIT License. See the `LICENSE` file for details.

## Citation

If you use this code or the methodology in your research, please cite the original paper:

```bibtex
@article{abdullah2025assessing,
  title={Assessing Financial Statement Risks among MCDM Techniques},
  author={Abdullah, Marwa and Anatolyevna, Revzon Oksana and Abdullah, Duaa},
  journal={arXiv preprint arXiv:2512.04035v1},
  year={2025}
}
```

For the implementation itself, you may cite this repository:
```
Chirinda, C. (2025). Heuristic-Augmented Financial Risk Index (HAFRI): An Open Source Implementation.
GitHub repository: https://github.com/chirindaopensource/assessing_financial_statement_risks
```

## Acknowledgments

-   Credit to **Marwa Abdullah et al.** for the foundational research that forms the entire basis for this computational replication.
-   This project is built upon the exceptional tools provided by the open-source community. Sincere thanks to the developers of the scientific Python ecosystem, including **Pandas, NumPy, and SciPy**.

--

*This README was generated based on the structure and content of the `assessing_financial_statement_risks_draft.ipynb` notebook and follows best practices for research software documentation.*

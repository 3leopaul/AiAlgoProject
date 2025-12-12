# Cost and Performance Optimization for Application Deployment and Scaling

**Authors:** Axel Juillard, Timothee Joliot, Léo-Paul Chauvigné

## Project Overview

This project addresses a critical MLOps challenge: optimizing cloud resource allocation for distributed machine learning training. The goal is to minimize infrastructure costs while ensuring training stability and meeting resource requirements.

We modeled this as a combinatorial optimization problem where we must decide:
1.  **Which VM types to rent** (from a catalogue of available VMs).
2.  **How many replicas of each application component to deploy**.

The optimization is constrained by:
*   **Resource Sufficiency:** Total CPU and RAM must meet the demands of all component replicas.
*   **System Stability:** Processing capacity (service rate) must exceed the incoming query rate (workload) to prevent bottlenecks.
*   **Availability:** We cannot rent more VMs of a specific type than are available in the catalogue.

## Data

The project uses two main datasets located in `OptionB_Data/`:
*   **`VM_Catalogue.xlsx`**: Defines the search space. Contains specs (CPU, RAM, Cost/hr, Max Quantity) for various VM types (e.g., T00, T01... T16).
*   **`Workload.xlsx`**: A time-series trace of incoming request rates ($\lambda$) that the system must handle.

## Approaches & Algorithms

We implemented and compared two main strategies:

### 1. Static Provisioning (Peak Load)
This strategy provisions enough resources to handle the *maximum* workload observed in the trace ($\lambda_{max}$) and keeps this configuration constant.

*   **Genetic Algorithm (Metaheuristic):** explored as a stochastic search method. In our experiments, it found valid but expensive solutions due to the large search space and limited generations.
*   **A* Search (Exact Algorithm):** Implemented to find the **optimal** static configuration.
    *   **Result:** Found a configuration costing **$1.5722/hr**, which exactly met the peak demand (25 CPU, 6.25 RAM) at the lowest possible price.

### 2. Elastic Provisioning (Dynamic Scaling)
This strategy adapts resources in real-time to match the fluctuating workload ($\lambda_t$).

*   **Greedy Heuristic:** A fast algorithm that attempts to satisfy the instantaneous resource demand by iteratively picking the most cost-efficient available VM.
    *   **Result:** Reduced total cost to **$1,398.47** over the simulation period, compared to **$1,979.40** for the optimal static solution.
    *   **Impact:** Achieved **~29.3% cost savings** by leveraging elasticity.

## Key Findings

*   **Elasticity Pays Off:** Dynamic scaling significantly reduces costs compared to static provisioning, even when using a simple greedy heuristic.
*   **Optimization Matters:** For static provisioning, the A* algorithm proved superior to the basic Genetic Algorithm configuration, finding the mathematically optimal mix of VMs.
*   **Heuristic Limitations:** The "Greedy" approach, while fast, can fall into local optima (the "Bus vs. Car" trap), potentially over-provisioning by selecting large, efficient VMs for small incremental needs.

## Usage

The main logic is contained in `notebook.ipynb`. To reproduce the results:
1.  Install dependencies: `pip install pandas numpy matplotlib openpyxl`
2.  Run the notebook cells sequentially to load data, define constraints, and execute the optimization algorithms.

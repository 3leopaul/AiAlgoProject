## Cost and Performance Optimization of a cloud cluster for a machine learning application

This project explores how to provision cloud VMs for a distributed ML training job while minimizing hourly cost and keeping the system stable.

### Data
- `OptionB_Data/VM_Catalogue.xlsx`: 17 VM types with CPU, RAM, hourly cost, and availability limits.
- `OptionB_Data/Workload.xlsx`: Time series of incoming request rates.
- Component requirements (per replica): CPU=1.0, RAM=0.25 GiB, service rates mu = [1416, 696, 1005, 1259] for 4 components.

### Notebook flow
1. Intro and problem context (resource sufficiency, stability, availability).
2. Load and sanity-check the datasets (columns, shapes, missing values).
3. Problem formulation:
   - Variables: replicas `r_i`, VM counts `nb_of_type_j_machine` (optional placement `y_{i,j,k}`).
   - Objective: minimize total hourly cost sum(cost_j * nb_of_type_j_machine).
   - Constraints: stability `r_i * mu_i > lambda_i`, CPU/RAM capacity, VM availability, optional per-VM packing.

### How to run
1. Install dependencies (inside your environment):
   ```bash
   pip install pandas openpyxl pulp # Already included inside a cell in the notebook
   ```
2. Open `notebook.ipynb` and run cells in order:
   - Data loading and sanity checks.



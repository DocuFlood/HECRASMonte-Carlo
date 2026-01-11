# HECRASMonte-Carlo  
## Detailed Workflow Description and Pseudo-Code

---

## 1. Overview

The **HECRASMonte-Carlo** workflow automates parameter modification, execution,
and post-processing of pre-configured **HEC-RAS unsteady flow models**.
The workflow is designed to support **sensitivity and uncertainty analyses**,
including **Monte Carlo–based experiments**, by systematically modifying model
inputs and extracting hydraulic outputs.

This document provides a **language-independent pseudo-code description**
of the workflow, enabling **independent re-implementation** without access to
the original source code.

---

## 2. Assumptions

- A valid **HEC-RAS model** (geometry, boundary conditions, and plans) already exists  
- **Manning’s roughness coefficients** are stored in a land-cover dataset  
- **Inflow hydrographs** are defined in unsteady flow input files  
- **Simulation outputs** are stored in HEC-RAS HDF result files  
- **Time step and simulation duration** are predefined in the HEC-RAS model  

---

## 3. Inputs

- One or more **HEC-RAS project files**  
- **Land-cover dataset** containing Manning’s roughness coefficients  
- External file with updated **Manning’s *n* values**  
- **Unsteady flow files** containing inflow hydrographs  
- External files with updated **hydrograph values**  
- **Simulation start and end times**  
- **Output directory** for exported results  

---

## 4. Outputs

- Updated **HEC-RAS model input files**  
- Completed **HEC-RAS simulations**  
- Time series of **flow**, **water surface elevation**, and **water depth**  
- Exported **plots and tabular data**  

---

## 5. Workflow Logic and Pseudo-Code

### **Algorithm: HECRASMonte-Carlo Workflow**

---

### **Stage 1 — Initialization and Configuration**

**Purpose**  
Prepare the computational environment and verify that all required inputs are
available before modifying the hydraulic model.

**Description**  
This stage ensures that the workflow starts from a clean state and that all
user-defined inputs (model files, parameter files, and output directories)
exist and are accessible.

**Pseudo-Code**
```text
BEGIN WORKFLOW

1. Initialization
   - Clear cached variables and release memory
   - Load user-defined configuration parameters
   - Verify existence of:
       • HEC-RAS project files
       • Land-cover datasets
       • Hydrograph input files
       • Output directories


### **Stage 2 — Update Manning’s Roughness Coefficients**

**Purpose** 
Systematically modify spatial roughness parameters to represent uncertainty in
land-surface characteristics.

**Description**
Manning’s roughness coefficients are read from an external file and used to
replace existing values in the land-cover dataset associated with the HEC-RAS
model. This enables rapid testing of multiple roughness scenarios without
manually editing the model.

### **Stage 3 — Update Inflow Hydrographs**

**Purpose**  
Introduce variability in boundary conditions by modifying inflow hydrographs.

**Description** 
Each unsteady flow file is updated by replacing its hydrograph values with new
values supplied externally. The original file structure and formatting are
preserved to ensure compatibility with HEC-RAS.

### **Stage 4 — Execute Hydraulic Simulations**

**Purpose**  
Automate the execution of multiple HEC-RAS simulations efficiently.

**Description** 
Each HEC-RAS project is opened and executed automatically. Simulation progress
is monitored until completion, after which results are saved. When multiple
projects are provided, simulations may be executed in parallel.

### **Stage 5 — Extract Flow Time Series**

**Purpose**  
Post-process simulation results to obtain discharge time series.

**Description** 
Flow results are extracted from HEC-RAS HDF output files. Time vectors are
constructed using simulation metadata, and results are exported for further
analysis and visualization.

### **Stage 6 — Extract Water Surface Elevation and Depth**

**Purpose**  
Analyze hydraulic responses at selected spatial locations.

**Description** 
Water surface elevation is extracted for user-defined computational cells.
Water depth is computed relative to initial conditions, enabling assessment
of temporal variations in inundation depth.

### **Stage 7 — Finalization**

**Purpose**  
Ensure a clean and stable workflow termination.

**Description** 
All open files are closed and system resources are released, allowing the
workflow to be repeated reliably for additional Monte Carlo realizations.

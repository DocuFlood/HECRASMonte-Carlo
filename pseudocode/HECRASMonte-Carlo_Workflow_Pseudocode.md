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


   - Validate number of roughness classes
   - Open land-cover dataset
   - Replace existing Manning’s n-values
   - Save and close dataset

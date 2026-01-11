# AutoRamtin: Detailed Pseudo-Code and Algorithmic Description

## 1. Purpose of the Workflow

The AutoRamtin workflow is designed to automate parameter modification,
execution, and post-processing of pre-configured HEC-RAS unsteady flow models.
The workflow supports sensitivity and uncertainty analysis, including
Monte Carlo–based experiments, by systematically modifying model inputs and
extracting hydraulic outputs.

This document provides a language-independent pseudo-code description of the
workflow, enabling independent re-implementation without access to the
original source code.

---

## 2. Assumptions

- A valid HEC-RAS model (geometry, boundary conditions, and plans) already exists
- Manning’s roughness coefficients are stored in a land-cover dataset
- Inflow hydrographs are defined in unsteady flow input files
- Simulation outputs are stored in HEC-RAS HDF result files
- Time step and simulation duration are predefined in the HEC-RAS model

---

## 3. Inputs

- One or more HEC-RAS project files
- Land-cover dataset containing Manning’s roughness coefficients
- External file with updated Manning’s n-values
- Unsteady flow files containing inflow hydrographs
- External files with updated hydrograph values
- Simulation start and end times
- Output directory for exported results

---

## 4. Outputs

- Updated HEC-RAS model input files
- Completed HEC-RAS simulations
- Time series of flow, water surface elevation, and water depth
- Exported plots and tabular data

---

## 5. Pseudo-Code Description

### Algorithm: AutoRamtin Workflow

BEGIN WORKFLOW

Initialization

Reset variables and clear memory

Load configuration parameters

Verify existence of required input files

Update Manning’s Roughness Coefficients

Read updated Manning’s n-values from external file

Validate number of roughness classes

Open land-cover dataset

Replace existing Manning’s n-values

Save and close dataset

Update Inflow Hydrographs
FOR each unsteady flow input file:
- Read updated hydrograph values
- Identify hydrograph section boundaries
- Replace existing values while preserving file structure
- Save updated unsteady flow file
END FOR

Execute Hydraulic Simulations
FOR each HEC-RAS project file:
- Open project
- Execute current simulation plan
- Monitor simulation until completion
- Save project and results
- Close HEC-RAS instance
END FOR
(Simulations may be executed in parallel)

Extract Flow Time Series
FOR each simulation output file:
- Read flow dataset from HDF output
- Construct time vector from simulation settings
- Align data lengths
- Generate flow plots
- Export flow time series to text files
END FOR

Extract Water Surface Elevation and Depth
FOR each simulation output file:
- Read water surface elevation dataset
- Select user-defined computational cell
- Extract water surface time series
- Compute water depth relative to initial condition
- Generate plots
- Export results to text files
END FOR

Finalization

Close all open files

Release system resources

END WORKFLOW


---

## 6. Reproducibility Statement

This pseudo-code provides a complete, language-independent description of the
AutoRamtin workflow. While implementation details may vary, the algorithmic
structure described here is sufficient to enable independent re-implementation.

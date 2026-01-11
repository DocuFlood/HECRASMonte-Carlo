Algorithm: HECRASMonte-Carlo Workflow
Stage 1 — Initialization and Configuration

Purpose:
Prepare the computational environment and verify that all required inputs are
available before modifying the hydraulic model.

Description:
This stage ensures that the workflow starts from a clean state and that all
user-defined inputs (model files, parameter files, and output directories)
exist and are accessible.

Pseudo-Code:

BEGIN WORKFLOW

1. Initialization
   - Clear cached variables and release memory
   - Load user-defined configuration parameters
   - Verify existence of:
       • HEC-RAS project files
       • Land-cover datasets
       • Hydrograph input files
       • Output directories
     
Stage 2 — Update Manning’s Roughness Coefficients

Purpose:
Systematically modify spatial roughness parameters to represent uncertainty in
land-surface characteristics.

Description:
Manning’s roughness coefficients are read from an external file and used to
replace existing values in the land-cover dataset associated with the HEC-RAS
model. This enables rapid testing of multiple roughness scenarios without
manually editing the model.

2. Update Manning’s Roughness Coefficients
   - Read updated Manning’s n-values from external file
   - Validate number of roughness classes
   - Open land-cover dataset
   - Replace existing Manning’s n-values
   - Save and close dataset

Stage 3 — Update Inflow Hydrographs

Purpose:
Introduce variability in boundary conditions by modifying inflow hydrographs.

Description:
Each unsteady flow file is updated by replacing its hydrograph values with new
values supplied externally. The workflow preserves the original file structure
and formatting to ensure compatibility with HEC-RAS.

3. Update Inflow Hydrographs
   FOR each unsteady flow input file:
       - Read updated hydrograph values
       - Identify hydrograph section boundaries
       - Replace existing values while preserving file structure
       - Save updated unsteady flow file
   END FOR

Stage 4 — Execute Hydraulic Simulations

Purpose:
Automate the execution of multiple HEC-RAS simulations efficiently.

Description:
Each HEC-RAS project is opened and executed automatically. Simulation progress
is monitored until completion, after which results are saved. When multiple
projects are provided, simulations may be executed in parallel to reduce
computational time.

4. Execute Hydraulic Simulations
   FOR each HEC-RAS project file:
       - Open project
       - Execute current simulation plan
       - Monitor simulation until completion
       - Save project and results
       - Close HEC-RAS instance
   END FOR
   (Simulations may be executed in parallel)

Stage 5 — Extract Flow Time Series

Purpose:
Post-process simulation results to obtain discharge time series.

Description:
Flow results are extracted from HEC-RAS HDF output files. Time vectors are
constructed using simulation metadata, and results are exported for further
analysis and visualization.

5. Extract Flow Time Series
   FOR each simulation output file:
       - Read flow dataset from HDF output
       - Construct time vector from simulation settings
       - Align data lengths
       - Generate flow plots
       - Export flow time series to text files
   END FOR
   
Stage 6 — Extract Water Surface Elevation and Depth

Purpose:
Analyze spatially explicit hydraulic responses at selected locations.

Description:
Water surface elevation is extracted for user-defined computational cells.
Water depth is computed relative to initial conditions, enabling assessment of
temporal variations in inundation depth.

Pseudo-Code:
6. Extract Water Surface Elevation and Depth
   FOR each simulation output file:
       - Read water surface elevation dataset
       - Select user-defined computational cell
       - Extract water surface time series
       - Compute water depth relative to initial condition
       - Generate plots
       - Export results to text files
   END FOR
   
Stage 7 — Finalization

Purpose:
Ensure a clean and stable workflow termination.

Description:
All open files are closed and system resources are released, allowing the
workflow to be repeated reliably for additional Monte Carlo realizations.

7. Finalization
   - Close all open files
   - Release system resources

END WORKFLOW


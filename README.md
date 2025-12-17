BEB Morphotypes Predictor
This repository provides a Python-based workflow to predict BEBs (Beaches in Estuaries and Bays) morphotypes (concave, linear, or convex) based on geomorphic and hydrodynamic variables. The notebook integrates multiple meaningful parameters into a single Morphotype Predictor Score (Yₘ) using a supervised linear weighting approach. You will find three notebooks, calibration, validation, and applicatione. Use the applicatione one. 

1. Overview of the Workflow
The process follows four main stages:
1.	Data input from an Excel file containing geomorphic and hydrodynamic variables.
2.	Computation of individual parameters describing sheltering, entrance geometry, and exposure, etc.
3.	Normalization and integration of parameters into a composite morphotypes predictor.
4.	Classification of BEBs morphotypes based on threshold values of Yₘ.

2. Input Data
The model reads data from an Excel file:
Data_BEBs.xlsx
Required sheet name: data_application
Required Columns
The following columns must be present in the Excel sheet:
•	BEBs – Beach name
•	SD – Dominant swell direction (degrees)
•	ED – Entrance direction (degrees)
•	Beach Aspect – Beach orientation (degrees)
•	LC (m) – Curvilinear coastline length
•	LS (m) – Straight-line coastline length
•	Barrier Effectiveness – Normalised barrier index (0–1)
•	Depth_m – Mean water depth (m)
•	Entrance Width (km) – Entrance width
•	Bay Area (km2) – Bay/estuary area
•	Distance_from_entrance – Distance of beach from entrance (m)
•	Fetch – Effective fetch length (m)
•	SH – Significant wave height or swell proxy

3. Parameter Definitions
A. Swell–Entrance Alignment (SEA)
Measures the angular alignment between the dominant swell direction and the bay/estuary entrance.
•	Computed using cosine similarity
•	Normalized between 0 and 1
•	Higher values indicate stronger swell penetration potential
B. Beach–Entrance Alignment (BEA)
Quantifies alignment between entrance orientation and beach aspect.
•	Indicates how directly wave energy can reach the beach
•	Normalized between 0 and 1
C. Integrated Coastal Sheltering (ICS)
Represents combined sheltering effects from coastline geometry and barriers.
•	Coastline straightness index (LC / LS)
•	Local Barrier Index (LBI)
•	Final ICS is the mean of normalized components
D. Mean Depth (MD)
Normalized mean depth of the bay/estuary.
•	Scaled between minimum (2 m) and maximum (60 m)
E. Entrance Width–Area Ratio (EWAR)
Indicates the openness of the entrance relative to bay size.
•	Defined as entrance width divided by square root of bay area
•	Normalized and classified into entrance types
F. Fetch-Adjusted Distance from Entrance (FADE)
Represents exposure based on beach position relative to the entrance and fetch.
•	Larger values indicate more sheltered inner locations
G. Swell Potentiality (SP)
Represents offshore wave energy potential impacting the system.
•	Normalized using observed minimum and maximum values

4. Morphotypes Predictor (Yₘ)
All parameters are combined using a linear weighted model:
Yₘ = Σ (ωᵢ · Xᵢ) + β₀
Where: - Xᵢ are normalized parameters (SEA, BEA, EWAR, FADE, MD, ICS, SP) - ωᵢ are regression-derived weights - β₀ is the intercept
Weights Used
Parameter	Weight
SEA	0.1487
BEA	0.5137
EWAR	0.0010
FADE	-0.9271
MD	0.0054
ICS	-0.1124
SP	0.0183
Intercept	0.4752

5. Morphotype Classification
Based on the Morphotype Predictor Score (Yₘ):
•	Yₘ < 0.40 → Mostly Concave to Concave
•	0.40 ≤ Yₘ ≤ 0.65 → Mostly Linear to Linear
•	Yₘ > 0.65 → Mostly Convex to Convex
These classes reflect increasing wave exposure and sediment delivery conditions.

6. Running the Code
1.	Place Data_BEBs.xlsx in the working directory.
2.	Update the directory path in the script if required.
3.	Run the script sequentially (Steps 0–4).
4.	The final output is a table containing all indices and the predicted morphotype class for each BEB.

7. Output
The final output includes:
•	Individual normalized indices (SEA, BEA, EWAR, FADE, ICS, MD, SP)
•	Composite Morphotype Predictor Score
•	Assigned Morphotype Class
These results can be used for comparative analysis across bays and estuaries and for morphodynamic interpretation.

8. Citation & Use
If you use or adapt this code, please cite the associated manuscript and acknowledge the methodology source.

9. Contact
For questions or collaboration, please contact:
Md Yousuf Gazi
Email: mdyousuf.gazi@sydney.edu.au

# MNO Mobility Intelligence: Residential Location Inference, Population Displacement Detection and Poverty Estimation

## 1. Project Overview

**MNO Mobility Intelligence** is an independent data science and machine learning project exploring how anonymised Mobile Network Operator (MNO) data can be transformed into useful geospatial and population-level insights for humanitarian response, development research and evidence-based intervention planning.

The project investigates how Call Detail Records (CDR) and Internet Protocol Detail Records (IPDR) can be used to:

- Infer subscribers' likely residential locations from observed mobile network activity.
- Classify recurring nighttime presence and distinguish stable residential patterns from uncertain observations.
- Identify potential population movement and relocation-like patterns.
- Analyse geographic population presence, activity concentration and origin–destination movements.
- Investigate how aggregated MNO mobility indicators could complement household survey data in poverty estimation.
- Evaluate model robustness, temporal consistency, parameter sensitivity and uncertainty where independently verified ground truth is limited.
- Develop privacy-conscious, reproducible analytical workflows that could inform future MNO data partnerships.

The project is designed around a practical challenge in mobile network analytics: mobile network records are generated for communication and network operations, not specifically for understanding residential behaviour or population movement.

Consequently, MNO data are incomplete observations of human activity. A subscriber's most frequently observed location is not necessarily their home, an absence of network activity does not necessarily mean that a person has moved, and a change in nighttime activity does not automatically indicate displacement.

The project therefore focuses on transparent assumptions, spatial and temporal analysis, uncertainty assessment and careful interpretation of analytical outputs.

**Project status:** Independent portfolio project. Not an operational MNO deployment.

**Geographic focus:** Kenya, using synthetic data.

**Primary technologies:** Python, Pandas, NumPy, Scikit-learn, GeoPandas, Matplotlib, Seaborn and geospatial analysis tools.

---

## 2. Project Objectives

The project has six primary objectives.

### 2.1 Residential Location Inference

Develop a reproducible approach for estimating a subscriber's likely residential location using recurring nighttime mobile network observations.

The methodology considers:

- Nighttime activity patterns.
- Frequency and consistency of observations.
- Spatial clustering of observed network activity.
- Cell tower coordinate quality.
- Minimum observation requirements.
- Alternative candidate locations.
- Confidence and uncertainty indicators.

### 2.2 Residence Classification

Classify inferred residential patterns according to the strength and consistency of observed evidence.

The classification distinguishes likely stable residential patterns from recurring but insufficiently established nighttime presence, weak evidence and cases where no reliable estimate can be produced.

### 2.3 Population Displacement Detection

Investigate potential movement by comparing subscriber activity across baseline and follow-up periods.

The analysis explores:

- Changes in dominant nighttime locations.
- Geographic distance between baseline and follow-up locations.
- Persistent activity in alternative locations.
- Potential relocation-like patterns.
- Temporary movement versus potentially persistent changes.
- Aggregate geographic inflows and outflows.

These outputs are exploratory movement indicators, not verified records of forced displacement.

### 2.4 Validation and Methodological Robustness

Evaluate how sensitive residential and movement estimates are to analytical assumptions.

This includes:

- Temporal holdout evaluation.
- Nighttime-window sensitivity.
- Minimum-observation thresholds.
- Dominant-location thresholds.
- Spatial clustering parameters.
- Method disagreement.
- Confidence-score diagnostics.
- Grid alignment sensitivity.
- No-ground-truth validation strategies.

### 2.5 Poverty Estimation and Survey Integration

Investigate whether aggregated MNO activity features could provide useful geographic information when combined with household survey indicators.

The project explores:

- Geographic aggregation of MNO features.
- Integration with household survey data.
- Geographic coverage and representativeness.
- Poverty classification.
- Household consumption estimation.
- Geographic train/test separation.
- Baseline comparisons and feature importance.

The current implementation uses synthetic survey data to test the analytical workflow. It does not establish a real-world relationship between MNO activity and household poverty.

### 2.6 Privacy-Conscious Analytical Design

Establish analytical practices that minimise unnecessary exposure of subscriber-level information.

These include:

- Pseudonymous subscriber identifiers.
- Geographic and temporal aggregation.
- Small-group suppression.
- Avoiding publication of subscriber-level mobility records.
- Avoiding publication of exact inferred residential coordinates.
- Separating analytical reference labels from inference features.
- Considering privacy-preserving computation methods in future extensions.

---

## 3. Dataset Description

### 3.1 Data Source

The project uses a synthetic Kenyan MNO dataset generated for analytical experimentation.

Two principal files are available:

| Dataset | Description |
|---|---|
| `MPD_sample_synthetic_ken_3000subs.csv` | Synthetic MNO sample dataset |
| `MPD_sample_synthetic_ken_3000subs_full.csv` | Full synthetic MNO dataset |

The full dataset contains approximately:

- **1,072,608 records**
- **3,000 unique subscribers**
- **35,956 unique cell IDs**
- **28 days of observations**
- **Observation period:** 1–28 February 2026
- **CDR records:** Approximately 94.14%
- **IPDR records:** Approximately 5.86%

### 3.2 Dataset Schema

The dataset contains the following fields:

| Field | Description |
|---|---|
| `msisdn` | Subscriber identifier |
| `datetime` | Date and time of the network event |
| `cell_id` | Mobile network cell identifier |
| `latitude` | Latitude associated with the cell observation |
| `longitude` | Longitude associated with the cell observation |
| `data_type` | Type of network record, such as CDR or IPDR |
| `service` | Network service associated with the record |
| `location_target` | Synthetic reference label associated with location |
| `location_place` | Synthetic location description |
| `movement_dist` | Simulator-generated movement distance |
| `movement_speed` | Simulator-generated movement speed |
| `is_robot` | Synthetic subscriber classification indicator |
| `profile` | Synthetic subscriber profile |
| `tourist` | Synthetic tourist indicator |
| `record_profile` | Synthetic record classification |
| `movement_profile` | Synthetic movement classification |

### 3.3 Important Data Leakage Consideration

Several columns are generated by the simulator and must not be used as model inputs when attempting to infer residential locations or movement patterns.

These include:

- `location_target`
- `location_place`
- `movement_dist`
- `movement_speed`
- `profile`
- `record_profile`
- `movement_profile`

Where appropriate, `location_target` may be used as a synthetic reference label for evaluation, but never as an inference feature.

This separation is important because using simulator-generated labels as model inputs would introduce target leakage and produce misleading evaluation results.

### 3.4 Data Limitations

The dataset is synthetic and does not represent actual Kenyan mobile network subscribers.

Its limitations include:

- Artificially generated subscriber behaviour.
- Potentially unrealistic communication patterns.
- Simulated cell tower distributions and coordinates.
- Limited observation period.
- No independently collected residential ground truth.
- No verified displacement labels.
- No real household poverty measurements linked to subscribers.
- Potential sampling and geographic coverage biases.

Results should therefore be interpreted as demonstrations of analytical methods and workflows rather than evidence of real-world predictive performance.

---

## 4. Project Architecture and Repository Structure

The project is organised into notebooks, data directories, reusable source code, documentation, reports, SQL and testing components.

```text
mno-mobility-intelligence/
│
├── README.md
├── LICENSE
├── requirements.txt
├── .gitignore
│
├── config/
│   └──
│
├── data/
│   ├── raw/
│   ├── interim/
│   ├── processed/
│   ├── external/
│   └── synthetic/
│
├── notebooks/
│   ├── 01_data_exploration_and_quality.ipynb
│   ├── 02_home_location_inference.ipynb
│   ├── 03_residence_classification.ipynb
│   ├── 04_population_displacement_detection.ipynb
│   ├── 05_validation_and_sensitivity_analysis.ipynb
│   └── 06_poverty_estimation_and_survey_integration.ipynb
│
├── src/
│   ├──
│
├── sql/
│   └──
│
├── dashboard/
│   └──
│
├── reports/
│   └──
│
├── docs/
│   └──
│
└── tests/
    └──
```

The notebook sequence represents the analytical workflow. Some repository directories are reserved for future modularisation and implementation.

### Data Directory

| Directory | Purpose |
|---|---|
| `data/raw/` | Original source datasets |
| `data/interim/` | Intermediate analytical datasets |
| `data/processed/` | Processed outputs from notebooks |
| `data/external/` | External geographic reference datasets |
| `data/synthetic/` | Synthetic datasets used for experimentation |

Raw, interim, processed and subscriber-level analytical outputs should not be committed to a public GitHub repository unless they have been explicitly reviewed and approved for publication.

---

## 5. Analytical Workflow

The project is divided into six analytical notebooks.

### Notebook 01: Data Exploration and MNO Data Quality Assessment

**File:** `01_data_exploration_and_quality.ipynb`

The first notebook establishes the quality, structure and characteristics of the MNO dataset before attempting residential or mobility inference.

#### Key activities

1. Load and inspect the synthetic MNO dataset.
2. Examine dataset dimensions and data types.
3. Assess missing values and duplicate records.
4. Validate timestamps and observation periods.
5. Examine CDR and IPDR distributions.
6. Analyse subscriber activity distribution.
7. Assess cell ID frequency and geographic distribution.
8. Investigate coordinate consistency for individual cell IDs.
9. Explore temporal activity patterns.
10. Examine geographic concentration and coverage.

#### Key analytical questions

- Are subscriber activity levels reasonably consistent, or is the dataset dominated by a relatively small group?
- How many unique subscribers and cell IDs are represented?
- Are cell IDs geographically consistent?
- How concentrated is activity across geographic locations?
- What proportion of records are CDR versus IPDR?
- Are there temporal patterns that could support residential inference?
- Are there data quality issues that could distort downstream analysis?

#### Outputs

The notebook establishes the data quality and coverage baseline used by subsequent analytical stages.

---

### Notebook 02: Home Location Inference

**File:** `02_home_location_inference.ipynb`

This notebook develops the initial residential location inference methodology.

The objective is to identify a subscriber's likely home area using recurring nighttime network activity.

#### Methodology

**1. Subscriber classification**

Separate ordinary subscribers from synthetic categories such as tourists and robot-like subscribers where appropriate.

These classifications are used to establish the analytical population and are not treated as proof of actual human behaviour.

**2. Nighttime activity extraction**

Extract observations within a nighttime window of:

- Start: 20:00
- End: 06:00

Observations occurring after midnight are assigned to the preceding nighttime period so that activity spanning midnight is treated as one night.

**3. Coordinate quality assessment**

Evaluate the geographic consistency of cell observations.

Cell IDs with inconsistent or unreliable coordinates are identified and excluded or handled separately.

**4. Candidate location aggregation**

Aggregate recurring nighttime activity by subscriber and candidate cell location.

Candidate locations are evaluated using measures such as:

- Observation frequency.
- Number of distinct observed nights.
- Recurrence across the observation period.
- Weighted nighttime activity.
- Geographic consistency.

**5. Spatial clustering**

Apply DBSCAN using Haversine distance to group nearby nighttime cell observations.

The initial experimental configuration includes:

| Parameter | Initial value |
|---|---:|
| Clustering algorithm | DBSCAN |
| Distance metric | Haversine |
| Epsilon | 1 km |
| Minimum samples | 2 |
| Minimum observed nights | 3 |

These values are experimental assumptions and are subsequently examined through sensitivity analysis.

**6. Fallback inference**

Where spatial clustering does not produce a suitable result, use a dominant candidate-location aggregation approach as a fallback.

**7. Confidence assessment**

Produce confidence indicators using observed activity consistency and candidate-location evidence.

These scores are heuristic indicators and should not automatically be interpreted as calibrated probabilities.

**8. Synthetic reference evaluation**

Where available, compare inferred locations against synthetic reference labels.

The reference labels are used for evaluation only.

#### Outputs

The notebook produces the following analytical files:

| Output | Description |
|---|---|
| `home_location_estimates.csv` | Inferred residential location estimates |
| `subscriber_classification.csv` | Subscriber analytical classifications |
| `nighttime_cell_candidates.csv` | Candidate nighttime locations |
| `cell_coordinate_validation.csv` | Cell coordinate quality assessment |

These are analytical outputs and should remain private when they contain subscriber-level information.

---

### Notebook 03: Residence Classification and Target-Area Presence Estimation

**File:** `03_residence_classification.ipynb`

This notebook extends home location inference by assessing the strength and consistency of residential evidence.

An inferred home location alone does not establish that a subscriber is a stable resident of a geographic area.

The notebook therefore separates residential location estimation from residence classification.

#### Methodology

1. Load inferred home locations and subscriber classifications.
2. Evaluate nighttime presence consistency.
3. Calculate observed activity across distinct calendar days.
4. Assess the number of observed nights and recurring activity.
5. Identify weak or insufficient residential evidence.
6. Identify recurring alternative nighttime locations.
7. Assign inferred coordinates to geographic administrative areas.
8. Analyse subscriber presence across target geographic areas.
9. Assess the distribution of estimates and uncertainty.

#### Residence classification framework

The analytical categories include:

| Classification | Interpretation |
|---|---|
| Likely stable resident | Relatively strong and recurring nighttime residential evidence |
| Recurring presence, not yet stable | Repeated activity without sufficient evidence to establish stable residence |
| Uncertain weak home presence | Limited or inconsistent evidence |
| Recurring alternative nighttime location | Evidence of another recurring nighttime location |
| Uncertain insufficient home estimate | Insufficient evidence to generate a reliable residential estimate |

A subscriber without an inferred home location must not automatically be classified as absent, displaced or living outside the study area.

#### Geographic analysis

The notebook uses administrative boundary data to associate inferred residential locations with geographic areas.

The project uses Kenya's 47 county boundaries obtained from an external geographic reference dataset.

The analysis examines geographic coverage and concentration.

An observed concentration of inferred locations in Nairobi, for example, should be interpreted in the context of the synthetic dataset and its sampling and coverage characteristics. It must not be interpreted as an estimate of Kenya's actual population distribution.

#### Illustrative analytical findings

At one stage of the analysis:

- Approximately 2,813 ordinary subscribers were retained.
- Approximately 2,569 subscribers had inferred home locations.
- This represented approximately 91.3% of the retained analytical population.
- Approximately 2,073 estimates came from the clustering approach.
- Approximately 496 estimates used the fallback method.
- Approximately 244 subscribers had no inferred home location.

These figures describe the synthetic dataset and the particular analytical configuration used at that stage. They are not population estimates.

#### Outputs

The notebook produces residence classification, geographic assignment and aggregate presence analyses for downstream use.

---

### Notebook 04: Population Displacement Detection

**File:** `04_population_displacement_detection.ipynb`

This notebook investigates whether changes in nighttime mobile activity can provide signals of potential population movement.

The analysis compares subscriber activity across baseline and follow-up periods.

#### Methodology

**1. Establish baseline residential patterns**

Use recurring nighttime activity during an initial observation period to estimate a baseline dominant location.

**2. Establish follow-up locations**

Infer dominant nighttime locations during a subsequent observation period.

**3. Calculate geographic movement**

Use Haversine distance to calculate the distance between baseline and follow-up locations.

**4. Identify potential movement**

Flag subscribers whose dominant observed nighttime location changes beyond an experimental geographic threshold.

**5. Distinguish movement patterns**

Investigate whether the observed change is consistent with:

- Temporary movement.
- Recurring activity in another area.
- A potentially persistent change in nighttime location.
- Insufficient follow-up evidence.

**6. Aggregate geographic movements**

Analyse movement patterns using geographic zones and origin–destination summaries.

**7. Examine geographic concentration**

Investigate areas with relatively high observed inflows, outflows and changes in activity.

#### Important methodological distinction

A change in dominant nighttime location is not sufficient evidence of forced displacement.

Other explanations include:

- Work-related travel.
- Temporary visits.
- Holidays.
- Changes in network usage.
- Changes in mobile network coverage.
- Device sharing.
- Incomplete observation.
- Changes in subscriber activity.

The project therefore describes these outputs as potential movement or relocation-like signals rather than confirmed displacement events.

#### Geographic aggregation

The notebook explores grid-based geographic aggregation to examine movement between areas.

Different grid sizes and offsets are considered because geographic boundaries can influence origin–destination counts.

Small groups may be suppressed to reduce disclosure risks. A minimum group-size threshold is an illustrative safeguard, not a complete privacy guarantee.

#### Outputs

The notebook supports subscriber-level analytical investigation and aggregate geographic movement summaries.

Subscriber-level outputs must not be published.

---

### Notebook 05: Validation, Sensitivity Analysis and Methodological Robustness

**File:** `05_validation_and_sensitivity_analysis.ipynb`

This notebook evaluates the reliability and stability of the residential inference and movement methodologies.

It addresses an important challenge in MNO analytics: residential ground truth is often unavailable, incomplete or expensive to collect.

Rather than relying on a single metric, the notebook examines temporal consistency, sensitivity to assumptions, method disagreement and uncertainty.

#### 1. Temporal holdout evaluation

Split the observation period into:

| Period | Dates | Purpose |
|---|---|---|
| Baseline | 1–14 February 2026 | Infer dominant residential location |
| Holdout | 15–28 February 2026 | Examine subsequent nighttime activity |

Compare baseline inferred locations with follow-up dominant locations.

Evaluate geographic consistency at different distance thresholds, including 1 km, 2 km and 5 km.

This tests temporal stability, not verified residential accuracy.

#### 2. Nighttime-window sensitivity

Compare alternative nighttime definitions:

- 19:00–05:00
- 20:00–06:00
- 21:00–05:00
- 22:00–06:00

Examine how changes in the observation window affect inferred locations and coverage.

#### 3. Minimum-observation sensitivity

Evaluate alternative minimum observation requirements:

- 3 nights
- 5 nights
- 7 nights
- 10 nights

This helps establish the trade-off between estimate coverage and evidence strength.

#### 4. Dominant-location threshold sensitivity

Evaluate alternative dominant-location thresholds:

- 0.30
- 0.40
- 0.50
- 0.60

Examine how the required proportion of activity at a candidate location affects inference results.

#### 5. Movement threshold sensitivity

Compare geographic movement thresholds:

- 3 km
- 5 km
- 10 km
- 15 km

This helps distinguish small geographic variations from more substantial changes in observed nighttime location.

#### 6. Method disagreement

Compare residential estimates from:

- DBSCAN spatial clustering.
- Dominant-cell or dominant-location aggregation.

Investigate where the methods agree or disagree and whether disagreement is associated with weaker evidence.

#### 7. Confidence-score diagnostics

Explore an evidence-based confidence score incorporating:

- Dominant-location share.
- Number of observed nights.
- Number of active weeks.

Compare the score against synthetic reference information where available.

A score should only be described as a probability if it has been appropriately calibrated and independently evaluated.

#### 8. No-ground-truth validation

Where verified home locations are unavailable, examine alternative diagnostics:

- Temporal consistency.
- Stability across parameter settings.
- Agreement between independent methods.
- Geographic plausibility.
- Observation coverage.
- Confidence distributions.
- Sensitivity to data filtering.

These diagnostics do not replace independent ground-truth validation.

#### 9. Geographic grid alignment sensitivity

Investigate whether origin–destination summaries change when geographic grid boundaries are shifted.

This helps identify artificial movement patterns caused by arbitrary geographic partitioning.

#### 10. Reproducibility

The notebook reconstructs the analytical objects it needs rather than relying on variables created in previous notebook kernels.

This supports independent execution and reduces hidden dependencies between notebooks.

---

### Notebook 06: Poverty Estimation and Survey Data Integration

**File:** `06_poverty_estimation_and_survey_data_integration.ipynb`

This notebook explores whether aggregated MNO mobility indicators could complement traditional household survey data in poverty estimation.

The underlying hypothesis is that patterns in mobile network activity may contain geographic information associated with economic activity, connectivity, population concentration and access to services.

However, MNO activity is not a direct measurement of household income or consumption.

The analysis therefore treats poverty estimation as an exploratory research problem requiring survey integration and independent validation.

#### 1. Geographic aggregation

Aggregate MNO activity into geographic grid zones.

The notebook derives features such as:

- Number of unique subscribers.
- Total network events.
- Number of active cell IDs.
- Number of active days.
- Nighttime activity.
- Daytime activity.
- Weekend activity.
- Nighttime activity ratio.
- Weekend activity ratio.
- Mean events per subscriber.
- Median events per subscriber.
- Mean active days per subscriber.
- Events per cell.
- Subscribers per cell.
- Events per active day.

These are aggregated geographic indicators, not direct household-level socioeconomic measurements.

#### 2. Small-group suppression

Zones with fewer than five unique subscribers are excluded from the main integrated analysis.

This is an illustrative minimum-group threshold. It does not by itself provide differential privacy or guarantee protection against re-identification.

#### 3. Synthetic household survey generation

Because verified household survey data and poverty labels are not available for this project, the notebook generates a synthetic survey dataset for pipeline testing.

The synthetic survey includes variables such as:

- Household size.
- Household consumption.
- Electricity access.
- Water access.
- Sanitation.
- Household assets.
- Education.
- Housing characteristics.

A synthetic poverty label is created using an illustrative monthly consumption threshold of 3,000 units.

This threshold is a demonstration parameter, not an official Kenyan poverty line.

#### 4. Geographic survey aggregation

Aggregate household survey indicators by geographic zone.

The resulting indicators include:

- Poverty rate.
- Mean household consumption.
- Median household consumption.
- Household size.
- Utility access.
- Other socioeconomic characteristics.

#### 5. MNO and survey integration

Join MNO geographic features with survey indicators using the shared geographic zone.

No subscriber identifier is linked to an individual household.

This is an ecological, geographic-level integration approach. It does not establish that individual subscribers correspond to surveyed households.

#### 6. Poverty classification

Develop a baseline classification workflow using aggregated MNO features.

The modelling workflow includes:

- Random Forest classifier.
- Dummy classifier baseline.
- Geographic group-based train/test separation.
- Precision.
- Recall.
- F1 score.
- ROC-AUC, where the test data support its calculation.
- Average precision.

#### 7. Household consumption estimation

Explore regression modelling to estimate aggregated consumption outcomes.

The workflow compares:

- Random Forest regression.
- Dummy regression baseline.

Evaluation metrics include:

- Mean Absolute Error (MAE).
- Root Mean Squared Error (RMSE).
- R-squared.

#### 8. Feature importance

Use permutation importance to examine which MNO indicators contribute most to model predictions.

Feature importance should be interpreted as model-specific evidence, not proof of a causal relationship between mobile activity and poverty.

#### 9. Geographic generalisation

Use geographic group-based splitting to avoid placing the same geographic zone in both training and testing sets.

This is important because MNO features are aggregated at zone level. Random household-level splitting can otherwise create overly optimistic evaluation results.

#### 10. Coverage and bias assessment

Examine how subscriber coverage, survey sample size and geographic representation vary across zones.

The analysis investigates whether apparent model performance may be influenced by uneven network coverage or survey sampling.

#### Important limitations

The poverty estimation workflow is currently a synthetic proof of concept.

- Survey records are simulated.
- Poverty labels are simulated.
- MNO activity is synthetic.
- The relationship between synthetic survey outcomes and MNO activity is not independently established.
- Results cannot be interpreted as real Kenyan poverty estimates.
- The analysis does not establish that MNO data can reliably identify poor households.

The purpose is to test the integration, modelling, validation and evaluation workflow before working with real household survey and MNO data.

---

## 6. Methodological Principles

The project follows several principles throughout its analytical workflow.

### 6.1 Separate inference from evaluation

Reference labels must not be used as inference features.

Synthetic labels may be used to evaluate an approach, but they must remain separate from the feature engineering and modelling process.

### 6.2 Distinguish absence of evidence from evidence of absence

A subscriber with insufficient nighttime observations should not automatically be classified as absent from an area.

Similarly, no inferred home location does not necessarily indicate displacement.

### 6.3 Account for temporal uncertainty

Residential and movement inference depend on observation windows.

A short observation period may capture temporary behaviour rather than a stable residential pattern.

### 6.4 Account for spatial uncertainty

Cell tower coordinates are not exact subscriber locations.

A subscriber connecting to a cell tower does not necessarily reside at the tower's coordinates.

Spatial estimates should therefore be interpreted as approximate areas of presence rather than precise addresses.

### 6.5 Prioritise interpretable methods

The initial methodology uses transparent statistical and spatial techniques, including frequency aggregation, dominant-location analysis, Haversine distance and DBSCAN.

These methods provide a practical foundation for evaluating assumptions before introducing more complex modelling approaches.

### 6.6 Treat confidence as evidence strength

Confidence scores are indicators of the strength of available evidence.

They are not automatically calibrated probabilities of a subscriber's true residential location.

### 6.7 Validate geographic generalisation

When developing geographic prediction models, training and test data should be separated by geographic unit where appropriate.

This reduces the risk of overstating performance through geographic leakage.

---

## 7. Technology Stack

| Technology | Application |
|---|---|
| Python | Main analytical programming language |
| Pandas | Data manipulation and aggregation |
| NumPy | Numerical computation |
| Scikit-learn | Machine learning and model evaluation |
| GeoPandas | Geographic data processing |
| Shapely | Spatial geometry operations |
| Matplotlib | Data visualisation |
| Seaborn | Statistical visualisation |
| SciPy | Statistical analysis |
| Jupyter Notebook | Exploratory analysis and documentation |
| Git | Version control |
| GitHub | Source code management and project documentation |

Additional tools and technologies may be introduced as the project develops.

---

## 8. Installation and Environment Setup

### Prerequisites

- Python 3.11
- Conda or Miniforge
- Git
- Jupyter Notebook or JupyterLab

### Clone the repository

```bash
git clone https://github.com/YOUR_USERNAME/mno-mobility-intelligence.git

cd mno-mobility-intelligence
```

Replace `YOUR_USERNAME` with the relevant GitHub username.

### Create the Conda environment

```bash
conda create -n mno-mobility python=3.11 -y
```

Activate it:

```bash
conda activate mno-mobility
```

### Install dependencies

```bash
pip install -r requirements.txt
```

If the dependency file has not yet been finalised, install the core packages:

```bash
pip install pandas numpy scipy scikit-learn
pip install matplotlib seaborn
pip install geopandas shapely pyproj
pip install jupyterlab notebook
```

### Launch Jupyter

```bash
jupyter lab
```

Open the notebooks directory and execute the notebooks in numerical order.

### Data setup

Place the synthetic MNO dataset in:

```text
data/raw/
```

The full dataset is expected at:

```text
data/raw/MPD_sample_synthetic_ken_3000subs_full.csv
```

Place the external administrative boundary data in:

```text
data/external/
```

Check the relevant notebook configuration cells if your local file paths differ.

---

## 9. Reproducibility and Execution Order

The recommended execution order is:

| Order | Notebook | Purpose |
|---:|---|---|
| 1 | Notebook 01 | Data exploration and quality assessment |
| 2 | Notebook 02 | Home location inference |
| 3 | Notebook 03 | Residence classification |
| 4 | Notebook 04 | Population displacement detection |
| 5 | Notebook 05 | Validation and sensitivity analysis |
| 6 | Notebook 06 | Poverty estimation and survey integration |

Each notebook should document its input files, configuration assumptions, processing steps and outputs.

Where a notebook depends on a previous notebook's exported files, those dependencies should be explicitly documented.

Notebook 05 also reconstructs key analytical objects needed for its experiments so that it does not depend solely on variables left in memory by Notebook 04.

For reproducibility:

- Fix random seeds where randomness is involved.
- Record analytical parameter values.
- Keep raw data separate from processed data.
- Avoid manually changing intermediate outputs.
- Clear notebook outputs before publishing.
- Document changes to methodology and assumptions.
- Record the software environment and dependency versions.

---

## 10. Privacy, Confidentiality and Responsible Data Use

Mobile network data can reveal sensitive information about people's daily routines, residential areas, movement and social behaviour.

Although the current project uses synthetic data, it is designed with privacy considerations that would be relevant to future real-world applications.

### Current design considerations

- Use pseudonymous subscriber identifiers.
- Avoid unnecessary subscriber-level data exposure.
- Aggregate observations geographically.
- Suppress small groups in aggregate reporting.
- Avoid publishing exact residential coordinates.
- Separate reference labels from inference features.
- Avoid treating inferred locations as verified identities or addresses.
- Restrict access to raw and intermediate datasets.
- Review exported outputs before publication.

### Future privacy-preserving work

Potential future extensions include:

**Differential privacy**

Explore mechanisms for adding calibrated statistical noise to aggregate outputs, with explicit privacy budgets and documented utility trade-offs.

**Private Set Intersection (PSI)**

Investigate how two parties could identify overlapping records without directly exchanging their underlying datasets.

**Secure Multi-Party Computation (MPC)**

Explore workflows in which multiple parties jointly compute agreed outputs without exposing their raw inputs to one another.

**Homomorphic encryption**

Investigate the feasibility and limitations of performing selected computations over encrypted data.

These are planned research and demonstration areas. They should not be interpreted as technologies already deployed in this project.

---

## 11. Current Limitations

The current project has several important limitations.

### Data limitations

- Uses synthetic rather than real MNO records.
- Limited to a 28-day observation period.
- Does not contain independently verified residential ground truth.
- Does not contain verified displacement labels.
- Does not contain real household poverty labels.
- Does not represent actual mobile network coverage or subscriber behaviour.

### Methodological limitations

- Nighttime activity is only a proxy for residential presence.
- Cell tower coordinates are approximate indicators of network activity.
- DBSCAN results depend on clustering parameters.
- Dominant-location methods may be affected by highly active subscribers.
- Confidence scores require further calibration.
- Temporal consistency is not equivalent to residential accuracy.
- Geographic grid boundaries can influence aggregate movement counts.
- Short observation periods make it difficult to distinguish temporary travel from long-term relocation.
- Synthetic poverty modelling cannot establish real-world predictive relationships.

### Operational limitations

The current work is an independent analytical prototype.

It has not yet been deployed within a mobile network operator's production environment, independently audited by an MNO engineering team or validated through a live humanitarian intervention.

---

## 12. Future Development Roadmap

The project is intended to evolve from a notebook-based analytical prototype into a reusable MNO mobility intelligence framework.

### Phase 1: Complete the analytical foundation

- Finalise and document Notebooks 01–06.
- Verify all analytical outputs.
- Consolidate methodology and parameter assumptions.
- Improve reproducibility.
- Add automated data quality checks.
- Review synthetic reference evaluation and geographic bias.

### Phase 2: Modularise the analytical workflows

Develop reusable Python modules for:

- MNO data ingestion.
- Data quality validation.
- Timestamp and timezone handling.
- Subscriber activity aggregation.
- Home location inference.
- Residence classification.
- Geographic assignment.
- Movement detection.
- Origin–destination analysis.
- Validation and reporting.

### Phase 3: Scalability and operator implementation

Develop a scalable implementation approach that can support larger datasets and different MNO data schemas.

Potential areas include:

- Efficient Pandas and PySpark processing.
- SQL-based analytical workflows.
- Configurable analytical parameters.
- Multi-operator data schema adapters.
- Incremental processing.
- Performance benchmarking.
- Reusable implementation templates.

### Phase 4: Privacy-preserving analytics

Develop demonstrations and technical documentation covering:

- Pseudonymisation.
- Geographic aggregation.
- Suppression.
- Differential privacy.
- Private Set Intersection.
- Secure Multi-Party Computation.
- Homomorphic encryption.

### Phase 5: Testing and technical review

Develop:

- Unit tests.
- Integration tests.
- Data validation tests.
- Geospatial calculation tests.
- Time-window boundary tests.
- SQL correctness tests.
- Performance tests.
- Code review exercises.
- Technical implementation documentation.

### Phase 6: Operational reporting and visualisation

Develop a dashboard to communicate aggregate insights, potentially including:

- Geographic subscriber coverage.
- Residential presence patterns.
- Residence classification distributions.
- Potential population movement.
- Geographic inflows and outflows.
- Data quality and confidence indicators.
- Coverage limitations and uncertainty.

All dashboard outputs should be aggregated and reviewed for privacy risks.

### Phase 7: Documentation and implementation guidance

Develop practical documentation for future MNO partnerships, including:

- Technical discovery questionnaires.
- Data requirements and schema documentation.
- Data minimisation guidance.
- Analytical implementation checklists.
- Parameter selection guidance.
- Data protection and privacy briefing materials.
- Operator engineering handover documentation.
- Training materials.

These activities are proposed future work, not completed operator engagements.

---

## 13. Intended Applications

The methods explored in this project could support research and analytical applications such as:

- Population presence estimation.
- Residential location inference.
- Human mobility analysis.
- Potential displacement monitoring.
- Geographic population change analysis.
- Humanitarian needs assessment.
- Target-area population estimation.
- Survey sampling and geographic representativeness analysis.
- Poverty estimation research.
- Evidence-based programme planning.

Any operational application would require appropriate data governance, legal basis, independent validation, privacy safeguards and careful consideration of the potential consequences of incorrect estimates.

---

## 14. Key Research Questions

The project is designed to investigate several broader questions:

1. How accurately can residential areas be inferred from sparse and irregular CDR/IPDR observations?
2. How does the choice of nighttime window affect residential inference?
3. How should minimum observation thresholds be selected when ground truth is unavailable?
4. How can spatial clustering and dominant-location approaches be combined?
5. How can confidence scores be made interpretable and calibrated?
6. How can temporary movement be distinguished from potentially persistent relocation?
7. How sensitive are population movement estimates to geographic aggregation choices?
8. How can MNO data be integrated with household survey data without linking individual subscribers to individual households?
9. Can aggregated mobile activity indicators improve geographic poverty estimation?
10. How can useful population-level insights be generated while minimising disclosure and re-identification risks?

---

## 15. Project Status Summary

| Component | Status |
|---|---|
| Synthetic MNO data exploration | Developed |
| Data quality assessment | Developed |
| Subscriber activity analysis | Developed |
| Cell coordinate validation | Developed |
| Nighttime activity extraction | Developed |
| Home location inference | Developed |
| Spatial clustering | Developed |
| Dominant-location fallback | Developed |
| Residential confidence indicators | Developed |
| Residence classification | Developed |
| Geographic county assignment | Developed |
| Population movement analysis | Developed / exploratory |
| Origin–destination analysis | Developed / exploratory |
| Temporal holdout validation | Implemented in Notebook 05 workflow |
| Parameter sensitivity analysis | Implemented in Notebook 05 workflow |
| Method disagreement analysis | Implemented in Notebook 05 workflow |
| Confidence diagnostics | Implemented in Notebook 05 workflow |
| Geographic grid sensitivity | Implemented in Notebook 05 workflow |
| MNO and synthetic survey integration | Developed in Notebook 06 workflow |
| Poverty classification experiment | Developed in Notebook 06 workflow |
| Consumption regression experiment | Developed in Notebook 06 workflow |
| Geographic model validation | Developed in Notebook 06 workflow |
| Reusable Python package | Planned |
| SQL implementation | Planned |
| PySpark scalability | Planned |
| Production operator integration | Not implemented |
| Dashboard | Planned |
| Automated testing framework | Planned |
| Differential privacy implementation | Planned |
| PSI/MPC/homomorphic encryption demonstrations | Planned |
| Operator technical review and workshops | Planned |

*Developed or implemented describes the analytical workflows and code prepared for the project. It does not imply that every experiment has been successfully executed, independently validated or deployed in production. Results should be reported only after reviewing actual notebook outputs.*

---

## 16. Conclusion

MNO Mobility Intelligence explores how mobile network records can be transformed into meaningful population-level insights through statistical modelling, geospatial analysis and careful treatment of uncertainty.

The work progresses from data quality assessment and residential location inference to residence classification, population movement analysis, methodological validation and exploratory poverty estimation.

A central theme throughout the project is that mobile network activity provides an incomplete representation of human behaviour. Reliable analysis therefore requires more than building predictive models: it requires understanding data generation processes, identifying biases, validating assumptions, communicating uncertainty and protecting the people represented in the data.

Although the current implementation uses synthetic data, it provides a foundation for developing reusable, interpretable and privacy-conscious analytical methods that could be evaluated further with appropriately governed real-world MNO and household survey datasets.

---

## 17. Disclaimer

This is an independent research and portfolio project developed using synthetic MNO and synthetic household survey data.

It is not affiliated with, commissioned by or deployed for any mobile network operator, government agency or humanitarian organisation.

No actual subscriber residential locations, displacement events or household poverty estimates are established by this project.

All findings derived from the synthetic datasets are illustrative and should not be interpreted as real-world population statistics or operational recommendations.
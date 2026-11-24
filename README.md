# MNO Mobility Intelligence: Residential Location Inference, Population Displacement Detection & Privacy-Preserving Analytics

![Python](https://img.shields.io/badge/Python-3.11-blue)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Analysis-150458)
![GeoPandas](https://img.shields.io/badge/GeoPandas-Geospatial-139C5A)
![Scikit Learn](https://img.shields.io/badge/Scikit--Learn-Machine%20Learning-F7931E)
![SQLite](https://img.shields.io/badge/SQLite-Database-003B57)
![Status](https://img.shields.io/badge/Project-Research%20%26%20Prototype-orange)
![Data](https://img.shields.io/badge/Data-Synthetic-lightgrey)

## 1. Project Overview

The **MNO Mobility Intelligence Project** is an end-to-end geospatial and spatio-temporal analytics project designed to investigate how Mobile Network Operator (MNO) records can be used to infer residential locations, estimate geographic presence, identify potential population displacement patterns, and explore the relationship between mobile network activity and socioeconomic conditions.

The project uses synthetic Call Detail Record (CDR) and Internet Protocol Detail Record (IPDR) data to develop and evaluate analytical methods that could potentially be adapted to real-world MNO environments.

The central analytical question is:

**Can mobile network activity provide sufficient evidence to estimate where subscribers reside, whether they are present in a particular geographic area over a defined period, and whether their mobility patterns suggest a change in residence?**

This question has important applications in:

- Humanitarian programme targeting.
- Population mobility and displacement monitoring.
- Geographic presence estimation.
- Emergency response planning.
- Poverty estimation and socioeconomic research.
- Evidence-based intervention design.
- Privacy-conscious population analytics.

The project progresses from exploratory data analysis and residential location inference to displacement detection, methodological validation, poverty estimation, operator-side implementation, and privacy-preserving analytics.

It consists of eight Jupyter notebooks, each addressing a distinct analytical or implementation component.

### Project objectives

The primary objectives are to:

1. Assess the quality, coverage and analytical suitability of MNO mobility records.
2. Develop methods for inferring subscriber residential locations using temporal and spatial activity patterns.
3. Classify subscribers based on the strength and consistency of residential evidence.
4. Estimate geographic presence across administrative and target areas.
5. Investigate potential population displacement through changes in subscriber mobility patterns.
6. Evaluate methodological robustness under sparse observations and limited ground truth.
7. Explore the integration of aggregated MNO activity features with household survey data for poverty estimation.
8. Develop scalable data processing workflows that could be adapted to operator infrastructure.
9. Demonstrate privacy-conscious approaches to MNO data processing and geographic analytics.
10. Document the assumptions, limitations, implementation considerations and privacy risks associated with mobility-based inference.

---

## 2. Dataset Description

### 2.1 Dataset overview

The project uses a synthetic Kenya-based MNO mobility dataset containing approximately:

| Dataset characteristic | Description |
|---|---:|
| Total records | 1,072,608 |
| Unique subscribers | 3,000 |
| Unique cell IDs | 35,956 |
| Observation period | 1–28 February 2026 |
| CDR records | Approximately 94.14% |
| IPDR records | Approximately 5.86% |
| Geographic coverage | Synthetic Kenya-based coordinates |

The dataset contains simulated subscriber activity and geographic observations over a 28-day period.

**Important:** This is synthetic data. It is not obtained from a real mobile network operator, does not represent actual subscriber behaviour, and must not be interpreted as evidence about the real population distribution or mobility patterns of Kenya.

### 2.2 Dataset files

The project uses two principal input files:

- `MPD_sample_synthetic_ken_3000subs.csv`
- `MPD_sample_synthetic_ken_3000subs_full.csv`

The full dataset is used for the main analytical workflows.

### 2.3 Dataset schema

The dataset contains the following fields:

| Field | Description |
|---|---|
| `msisdn` | Synthetic subscriber identifier |
| `datetime` | Timestamp associated with the network activity |
| `cell_id` | Cell identifier associated with the event |
| `latitude` | Geographic latitude of the associated cell |
| `longitude` | Geographic longitude of the associated cell |
| `data_type` | Type of network record, such as CDR or IPDR |
| `service` | Recorded service or event category |
| `location_target` | Simulator-generated location reference |
| `location_place` | Simulator-generated place category |
| `movement_dist` | Simulator-generated movement distance |
| `movement_speed` | Simulator-generated movement speed |
| `is_robot` | Simulator-generated classification |
| `profile` | Simulator-generated subscriber profile |
| `tourist` | Simulator-generated tourist classification |
| `record_profile` | Simulator-generated record classification |
| `movement_profile` | Simulator-generated movement classification |

### 2.4 Prevention of target leakage

An important methodological consideration is separating observed network information from simulator-generated labels.

The following fields are not used as predictive or inference features:

- `location_target`
- `location_place`
- `movement_dist`
- `movement_speed`
- `is_robot`
- `profile`
- `tourist`
- `record_profile`
- `movement_profile`

These fields contain information generated by the synthetic data simulator and could introduce target leakage if used to infer residential locations or subscriber movement.

Where appropriate, `location_target` may be used as a synthetic reference for evaluation. It is not treated as independently verified ground truth.

This separation helps ensure that the inference process relies on the observable network activity rather than information that would not necessarily be available in an actual operator dataset.

---

# 3. Project Architecture and Notebook Structure

The project is organised into eight analytical notebooks.

| Notebook | Name | Main objective |
|---|---|---|
| 01 | MNO Data Exploration and Quality Assessment | Understand the data, coverage and limitations |
| 02 | Residential Location Inference | Infer likely subscriber residential locations |
| 03 | Residence Classification and Geographic Presence Estimation | Classify residential evidence and estimate geographic presence |
| 04 | Population Displacement Detection | Identify potential changes in residential location and mobility patterns |
| 05 | Validation, Sensitivity Analysis and Methodological Robustness | Evaluate inference reliability and parameter sensitivity |
| 06 | Poverty Estimation and Survey Data Integration | Explore geographic MNO features and household survey integration |
| 07 | Scalability and Operator Implementation | Demonstrate operator-oriented processing and implementation workflows |
| 08 | Privacy-Preserving MNO Analytics | Explore privacy-conscious processing and privacy-enhancing technologies |

The notebooks are designed to build on one another, moving from data understanding to analytical inference, validation, operational considerations and privacy.

---

# 4. Detailed Notebook Documentation

## Notebook 01 — MNO Data Exploration and Quality Assessment

**File:** `01_mno_data_exploration_and_quality_assessment.ipynb`

### Objective

The first notebook establishes an understanding of the MNO dataset before any residential location inference is attempted.

The purpose is to determine whether the available records contain sufficient temporal, geographic and subscriber-level information to support mobility analysis.

The analysis focuses on data quality, observation coverage, subscriber activity, temporal behaviour, geographic consistency and the limitations of the available observations.

### 4.1 Data loading and schema assessment

The notebook loads the synthetic MNO dataset and performs initial structural checks.

Activities include:

- Loading the full CDR/IPDR dataset.
- Inspecting dataset dimensions.
- Reviewing column names and data types.
- Identifying missing values.
- Checking duplicate records.
- Converting timestamps into appropriate datetime formats.
- Identifying unique subscribers and cell IDs.
- Reviewing the observation period.

### 4.2 MNO record distribution

The analysis examines the distribution of network record types.

The dataset contains both:

- Call Detail Records (CDR).
- Internet Protocol Detail Records (IPDR).

The analysis investigates the relative contribution of each record type and considers how different event types may affect the interpretation of subscriber activity.

### 4.3 Subscriber activity analysis

Subscriber-level activity is examined to determine whether observations are distributed consistently or dominated by a smaller group of highly active subscribers.

The analysis includes:

- Events per subscriber.
- Distribution of subscriber activity.
- Active days per subscriber.
- Subscriber observation coverage.
- Variability in subscriber activity.
- Identification of low-activity and high-activity subscribers.

This is important because residential location inference can become unreliable when some subscribers have substantially fewer observations than others.

### 4.4 Temporal activity analysis

The notebook investigates when subscribers generate network events.

Activities include:

- Hourly event distribution.
- Daily event distribution.
- Weekday versus weekend activity.
- Daytime versus nighttime activity.
- Temporal coverage across the observation period.

These patterns help establish whether there is sufficient nighttime activity to support residential location inference.

### 4.5 Geographic and cell-site analysis

The geographic assessment examines cell ID distribution and coordinate consistency.

Activities include:

- Unique cell ID analysis.
- Latitude and longitude validation.
- Geographic coordinate distribution.
- Cell-site observation frequency.
- Geographic concentration of activity.
- Identification of invalid or inconsistent coordinates.

The analysis also considers the possibility that different records associated with the same cell ID may contain inconsistent coordinates.

### 4.6 Data quality considerations

The notebook identifies several issues that may affect subsequent inference:

- Uneven subscriber activity.
- Sparse observations.
- Incomplete temporal coverage.
- Geographic concentration.
- Inconsistent cell coordinates.
- Different record-generation frequencies.
- Potential limitations in interpreting cell-level observations as precise subscriber locations.

### Deliverables

Notebook 01 establishes the data quality baseline and informs the analytical decisions made in subsequent notebooks.

**Key outcome:** A structured understanding of the available MNO records and their suitability for residential location inference and mobility analysis.

---

## Notebook 02 — Residential Location Inference

**File:** `02_home_location_inference.ipynb`

### Objective

This notebook develops methods for estimating subscriber residential locations using recurring nighttime mobile network activity.

The underlying assumption is that subscribers are more likely to generate recurring activity around their residential locations during nighttime periods than during daytime periods.

This is an inference assumption, not a guarantee that nighttime activity always represents a subscriber's home.

### 4.7 Subscriber classification

The notebook begins by separating ordinary subscriber activity from other simulated activity categories.

The intention is to reduce the influence of records that may not represent normal subscriber mobility.

Simulator-generated fields are not used as inference features.

### 4.8 Nighttime activity filtering

A nighttime observation window is applied:

- Start: 20:00.
- End: 06:00.

Because the observation window crosses midnight, records after midnight are assigned to the preceding nighttime observation period.

This prevents one continuous nighttime period from being incorrectly treated as two unrelated calendar nights.

### 4.9 Residential candidate generation

For each subscriber, nighttime events are aggregated to identify candidate residential locations.

The analysis considers:

- Number of nighttime observations.
- Frequency of visits to candidate locations.
- Number of distinct nights with activity.
- Consistency of recurring locations.
- Geographic coordinates associated with the observations.

The purpose is to identify locations with recurring nighttime activity rather than simply selecting the most frequently observed cell.

### 4.10 Spatial clustering using DBSCAN

The notebook implements DBSCAN to group geographically nearby nighttime observations.

The clustering process uses geographic distance calculations based on the Haversine formula.

The main configuration includes:

| Parameter | Demonstration setting |
|---|---:|
| Clustering algorithm | DBSCAN |
| Distance metric | Haversine |
| Epsilon | 1 km |
| Minimum samples | 2 |
| Minimum nighttime observations | 3 nights |

The clustering approach helps account for situations where nearby cell sites may represent the same general residential area.

### 4.11 Alternative candidate aggregation

DBSCAN may not produce a reliable residential cluster for every subscriber, particularly where observations are sparse.

An alternative aggregation method is therefore used to provide a fallback estimate where the clustering approach is insufficient.

This allows the methodology to retain useful candidate evidence without treating every subscriber as if they have a sufficiently dense spatial observation history.

### 4.12 Residential location scoring

Candidate residential locations are assessed using evidence from recurring nighttime activity.

The scoring considers factors such as:

- Frequency of observations.
- Nighttime presence.
- Number of distinct observation nights.
- Consistency of activity.
- Relative strength of competing candidate locations.

The resulting scores are used to rank candidate locations and support confidence assessment.

### 4.13 Synthetic reference evaluation

The inferred residential locations are compared with available simulator-generated reference labels.

This provides a way to examine the behaviour of the inference methodology under controlled synthetic conditions.

However, these comparisons are not independent real-world validation.

### 4.14 Outputs

The notebook generates intermediate outputs used in subsequent notebooks, including:

- `home_location_estimates.csv`
- `subscriber_classification.csv`
- `nighttime_cell_candidates.csv`
- `cell_coordinate_validation.csv`

These files contain analytical outputs and should remain local rather than being committed to a public repository.

**Key outcome:** A residential location inference workflow using nighttime activity, spatial clustering, candidate aggregation and evidence-based scoring.

---

## Notebook 03 — Residence Classification and Geographic Presence Estimation

**File:** `03_residence_classification_and_geographic_presence.ipynb`

### Objective

Notebook 03 extends residential location inference by assessing the strength and consistency of the inferred residential evidence.

Rather than treating every subscriber with a candidate residential location as a confirmed resident, the notebook introduces residence classifications that account for uncertainty and incomplete observations.

### 4.15 Residence classification

Subscribers are classified using the strength of their inferred nighttime residential evidence.

The classification framework distinguishes between:

- Likely stable resident.
- Recurring presence that is not yet stable.
- Uncertain or weak home presence.
- Possible multiple residential areas.
- Insufficient observations.
- Insufficient residential location estimates.

This approach is important because the absence of a reliable home estimate does not necessarily mean that a subscriber is absent from a geographic area.

### 4.16 Evidence strength

Residential evidence is grouped into broad strength categories.

The classification considers:

- Number of observations.
- Number of nights with activity.
- Consistency of recurring nighttime locations.
- Relative concentration of observations.
- Availability of a reliable residential estimate.

The notebook uses rule-based evidence thresholds rather than treating the resulting confidence score as a calibrated probability.

### 4.17 Geographic boundary integration

The notebook integrates geographic administrative boundaries to associate inferred residential locations with administrative areas.

The analysis uses geospatial processing to:

- Load administrative boundary data.
- Validate geographic coordinates.
- Perform spatial joins.
- Associate inferred residential locations with geographic areas.
- Aggregate subscriber presence by administrative unit.

The project uses Kenya administrative boundaries, including county-level geographic information.

### 4.18 Geographic presence estimation

The analysis aggregates inferred residential locations to estimate the distribution of subscribers across geographic areas.

The synthetic analysis produced the following illustrative results at one stage:

| Metric | Result |
|---|---:|
| Ordinary subscribers | 2,813 |
| Subscribers with home estimates | 2,569 |
| Home estimates generated | Approximately 91.3% |
| DBSCAN-based estimates | 2,073 |
| Alternative-method estimates | 496 |
| Subscribers without estimates | 244 |

The geographic assignment showed a strong concentration of inferred locations in Nairobi:

| County | Inferred residential locations |
|---|---:|
| Nairobi | 2,264 |
| Kiambu | 213 |
| Machakos | 74 |
| Kajiado | 18 |

These results describe the synthetic dataset and its generation or coverage characteristics. They must not be interpreted as estimates of Kenya's real population distribution.

### 4.19 Methodological considerations

The notebook explicitly considers:

- Insufficient observation coverage.
- Multiple recurring nighttime locations.
- Geographic boundary assignment.
- Uncertain residential evidence.
- Potential spatial filtering bias.
- Differences between inferred residence and geographic presence.

**Key outcome:** A residence classification and geographic aggregation framework that distinguishes strong evidence from uncertain or insufficient observations.

---

## Notebook 04 — Population Displacement Detection

**File:** `04_population_displacement_detection.ipynb`

### Objective

This notebook investigates whether changes in subscriber mobility patterns can be used to identify potential changes in residential location.

The analytical motivation is to explore how MNO data could support population mobility monitoring and emergency response planning.

A change in inferred residential location may indicate relocation, but it does not independently establish that forced displacement has occurred.

### 4.20 Baseline residence estimation

The analysis first establishes a baseline residential location for each eligible subscriber.

The baseline is used as a reference for subsequent observations.

The process considers:

- Baseline observation period.
- Nighttime activity.
- Minimum observation requirements.
- Dominant geographic location.
- Residential evidence consistency.

### 4.21 Follow-up location estimation

A follow-up period is used to examine whether subscriber activity shifts away from the baseline residential area.

The analysis estimates dominant follow-up locations using the available geographic observations.

### 4.22 Movement distance calculations

The Haversine formula is used to estimate geographic distance between baseline and follow-up locations.

The analysis considers configurable movement thresholds to identify potentially meaningful geographic shifts.

### 4.23 Temporary movement versus potential relocation

Not every movement away from a baseline residential location represents a permanent change of residence.

The notebook therefore explores distinctions between:

- Temporary mobility.
- Recurring movement.
- Persistent presence in a new geographic area.
- Potential residential relocation.
- Insufficient follow-up observations.

The methodology relies on repeated evidence rather than a single movement event.

### 4.24 Origin-destination analysis

The notebook explores geographic movement between baseline and follow-up locations.

The objective is to understand:

- Where subscribers originate.
- Where they subsequently appear.
- Which geographic areas experience potential inflows.
- Which geographic areas experience potential outflows.
- Whether movement patterns are geographically concentrated.

### 4.25 Geographic displacement patterns

The analysis investigates potential geographic hotspots and changes in activity distribution.

This provides an exploratory foundation for identifying locations that may require additional investigation.

### 4.26 Important limitations

The available dataset covers only 28 days.

Consequently:

- There is limited historical baseline information.
- Seasonal mobility cannot be adequately assessed.
- Short-term movement may be confused with longer-term relocation.
- Forced displacement cannot be distinguished reliably from voluntary movement.
- Empty origin-destination outputs may reflect minimum thresholds or sparse observations rather than an absence of movement.

The outputs should therefore be described as potential relocation or displacement-related signals, not confirmed forced displacement.

**Key outcome:** An exploratory framework for detecting changes in residential mobility patterns using baseline and follow-up observations.

---

## Notebook 05 — Validation, Sensitivity Analysis and Methodological Robustness

**File:** `05_validation_sensitivity_and_methodological_robustness.ipynb`

### Objective

Notebook 05 evaluates the robustness of the residential inference methodology and examines how methodological choices affect the resulting estimates.

A major challenge in MNO analytics is that independently verified residential ground truth is often unavailable.

This notebook therefore explores validation methods that do not depend entirely on known residential labels.

### 4.27 Temporal holdout validation

The observation period is divided into separate baseline and holdout periods.

| Period | Dates |
|---|---|
| Baseline | 1–14 February 2026 |
| Holdout | 15–28 February 2026 |

Residential locations are inferred using baseline observations and compared with dominant locations observed during the holdout period.

The analysis examines geographic consistency at different distance tolerances.

This evaluates temporal stability, not independently verified residential accuracy.

### 4.28 Nighttime window sensitivity

The notebook investigates whether alternative nighttime definitions change residential location estimates.

The tested windows include:

- 19:00–05:00.
- 20:00–06:00.
- 21:00–05:00.
- 22:00–06:00.

The purpose is to understand how the choice of observation window affects candidate locations and inference stability.

### 4.29 Parameter sensitivity analysis

The methodology is evaluated across different parameter configurations.

Examples include:

| Parameter | Tested values |
|---|---|
| Minimum observed nights | 3, 5, 7, 10 |
| Dominant location share | 0.3, 0.4, 0.5, 0.6 |
| Movement distance | 3, 5, 10, 15 km |
| DBSCAN epsilon | 1 km demonstration setting |
| Minimum DBSCAN samples | 2 demonstration setting |

The analysis investigates how changing these parameters affects:

- Number of estimated residences.
- Number of uncertain subscribers.
- Geographic stability.
- Method agreement.
- Potential movement classifications.

### 4.30 Method comparison

The notebook compares DBSCAN-based spatial clustering with dominant-location aggregation.

The objective is to understand where the methods agree and where their estimates differ.

Method disagreement is treated as useful diagnostic information rather than automatically assuming one method is correct.

### 4.31 Confidence and uncertainty assessment

The analysis explores evidence-based confidence scores using factors such as:

- Dominant location share.
- Number of observed nights.
- Number of active weeks.
- Consistency of recurring activity.

The resulting score is an evidence-strength indicator.

It should not be interpreted as a probability of correctly identifying a subscriber's home unless it is calibrated against suitable independent reference data.

### 4.32 No-ground-truth diagnostics

The notebook examines analytical signals that can be used when verified residential labels are unavailable.

These include:

- Temporal stability.
- Parameter sensitivity.
- Method agreement.
- Geographic plausibility.
- Observation coverage.
- Distribution of confidence scores.
- Proportion of subscribers without reliable estimates.

### 4.33 Geographic grid sensitivity

The notebook investigates how geographic aggregation changes when grid size and grid alignment are modified.

This is important because arbitrary grid boundaries can influence the number of subscribers assigned to geographic zones and the interpretation of movement patterns.

### 4.34 Validation limitations

Temporal consistency does not prove residential accuracy.

Similarly, agreement between two inference methods does not establish that either method is correct.

The notebook therefore distinguishes:

- Synthetic reference evaluation.
- Temporal stability.
- Sensitivity analysis.
- Methodological consistency.
- Independent real-world validation.

**Key outcome:** A validation and robustness framework for understanding uncertainty, parameter dependence and the limitations of inference without ground truth.

---

## Notebook 06 — Poverty Estimation and Survey Data Integration

**File:** `06_poverty_estimation_and_survey_data_integration.ipynb`

### Objective

This notebook explores whether aggregated MNO activity features can be integrated with household survey information to support geographic poverty estimation.

The objective is not to claim that mobile network activity directly measures poverty.

Instead, the notebook investigates the analytical workflow required to combine geographic mobile activity indicators with socioeconomic survey outcomes.

### 4.35 Geographic aggregation

MNO observations are assigned to geographic grid zones.

The analysis aggregates activity at the geographic level rather than linking individual subscribers to household records.

This creates an analytical foundation for combining MNO features with geographic survey outcomes.

### 4.36 MNO feature engineering

The notebook develops geographic activity indicators, including:

- Unique subscribers per geographic zone.
- Total network events.
- Number of observed cell sites.
- Number of active days.
- Nighttime event counts.
- Daytime event counts.
- Weekend activity.
- Nighttime activity ratios.
- Daytime activity ratios.
- Mean events per subscriber.
- Median events per subscriber.
- Mean active days.
- Events per cell.
- Subscribers per cell.
- Events per active day.

These variables provide different measures of network activity and geographic coverage.

### 4.37 Geographic privacy and minimum-group suppression

Before geographic outputs are used in the survey integration workflow, the notebook applies a minimum subscriber threshold.

The demonstration uses a minimum group size of five subscribers for relevant geographic aggregates.

This is intended to reduce the disclosure risk associated with very small geographic groups.

However, minimum-group suppression alone does not guarantee anonymity or prevent all disclosure attacks.

### 4.38 Synthetic household survey generation

Because verified household survey and poverty labels are not available in the synthetic MNO dataset, the notebook generates a separate synthetic household survey for testing the integration workflow.

The generated variables include:

- Household size.
- Household consumption.
- Electricity access.
- Water access.
- Sanitation access.
- Household assets.
- Education indicators.
- Housing characteristics.
- Synthetic poverty classification.

An illustrative monthly poverty threshold is used for the demonstration.

The threshold is not presented as an official Kenyan poverty line.

The synthetic survey is not a real household survey and is not derived from the MNO subscribers.

### 4.39 Geographic survey aggregation

Survey outcomes are aggregated by geographic zone.

Examples include:

- Number of surveyed households.
- Average household consumption.
- Median household consumption.
- Average household size.
- Geographic poverty rate.
- Access to electricity.
- Access to water.
- Sanitation indicators.
- Household asset indicators.

### 4.40 MNO and survey integration

The aggregated MNO features are joined with aggregated survey outcomes using geographic zone identifiers.

The integration does not use subscriber-level identifiers to match individual subscribers to individual households.

This is important because the purpose is to explore geographic-level associations rather than individual-level household identification.

### 4.41 Poverty classification

The notebook develops a baseline classification workflow to investigate whether aggregated MNO features can distinguish between geographic zones with different synthetic poverty outcomes.

The analysis includes:

- Random Forest classification.
- Dummy classifier baseline.
- Geographic train/test separation.
- Precision.
- Recall.
- F1-score.
- ROC-AUC.
- Average precision.
- Feature importance analysis.

Geographic separation is used to reduce the risk of information leakage caused by placing observations from the same geographic zone in both training and testing data.

### 4.42 Consumption prediction

A regression workflow is also explored to estimate synthetic household consumption outcomes at the geographic level.

The analysis compares a Random Forest regressor with a dummy baseline.

Evaluation metrics include:

- Mean Absolute Error (MAE).
- Root Mean Squared Error (RMSE).
- R-squared.

### 4.43 Geographic performance and representativeness

The notebook examines whether model performance differs across geographic areas and whether coverage limitations influence the results.

The analysis considers:

- Number of MNO observations per zone.
- Number of surveyed households per zone.
- Geographic representation.
- Coverage differences.
- Potential bias in the availability of mobile activity observations.

### 4.44 Critical interpretation

The poverty estimation workflow is experimental.

Because both the survey outcomes and poverty labels are synthetic, the results cannot establish that MNO activity predicts real-world poverty.

The notebook demonstrates a geographic data integration and modelling pipeline that could be adapted to real household survey data after appropriate validation, sampling design considerations and poverty measurement calibration.

**Key outcome:** An experimental framework for combining aggregated MNO activity features with geographic household survey outcomes for poverty-related analysis.

---

## Notebook 07 — Scalability and Operator Implementation

**File:** `07_scalability_and_operator_implementation.ipynb`

### Objective

Notebook 07 explores how the analytical methods developed in the preceding notebooks could be structured for implementation within an MNO's data environment.

The objective is to move beyond notebook-based analysis and demonstrate reusable data processing components, schema adaptation, batch processing, geographic aggregation and operator-oriented documentation.

In a real MNO environment, analytical methods may need to run on operator infrastructure because of data confidentiality, security and data-sharing constraints.

### 4.45 Configurable data schema

The notebook introduces a configurable schema adapter to map operator-specific field names to a canonical analytical schema.

This allows the processing workflow to accommodate differences in source field names and data structures.

The canonical fields include:

- Subscriber identifier.
- Event timestamp.
- Cell identifier.
- Latitude.
- Longitude.
- Data type.
- Service category.

The schema adapter provides a foundation for adapting the same analytical workflow to different input formats.

### 4.46 Data quality validation

Reusable validation functions are developed to check:

- Required columns.
- Timestamp validity.
- Geographic coordinate validity.
- Missing identifiers.
- Duplicate records.
- Invalid observations.
- Schema compatibility.

This helps identify problems before records enter the geographic aggregation workflow.

### 4.47 Geographic grid assignment

The notebook implements reusable geographic grid assignment to associate observations with geographic zones.

This allows large event datasets to be aggregated spatially without requiring every analytical operation to be performed at the raw event level.

### 4.48 Chunk-based processing

The workflow processes the input CSV in configurable chunks rather than loading the entire dataset into memory.

The demonstration uses a configurable chunk size of 100,000 records.

The processing pipeline:

1. Reads a chunk of MNO records.
2. Applies schema standardisation.
3. Validates the records.
4. Processes timestamps and coordinates.
5. Assigns geographic zones.
6. Aggregates relevant activity.
7. Writes intermediate aggregation results to SQLite.

This provides a demonstration of how large event datasets can be processed incrementally.

### 4.49 SQLite aggregation architecture

The notebook creates a local SQLite database to store intermediate aggregation tables.

The database structure includes:

- `zone_events`
- `subscriber_zone`
- `subscriber_zone_day`
- `zone_cell`
- `zone_active_day`

These tables support intermediate processing and the generation of geographic-level activity indicators.

The database contains subscriber-level intermediate records and must therefore be treated as private working data.

### 4.50 Geographic feature generation

The notebook generates geographic activity indicators from the aggregated database.

Examples include:

- Unique subscribers.
- Total events.
- Number of observed cells.
- Active days.
- Nighttime activity.
- Daytime activity.
- Weekend activity.
- Subscriber activity statistics.

### 4.51 Minimum-group suppression

Geographic aggregates are subjected to a minimum subscriber threshold before being included in the relevant outputs.

The demonstration uses a configurable minimum group size of five subscribers.

This is an illustrative disclosure-risk reduction measure, not a complete privacy guarantee.

### 4.52 SQL implementation examples

The notebook includes SQL-style aggregation examples to demonstrate how the analytical logic could be translated into database queries.

The examples provide a starting point for implementation discussions with data engineers.

They should be adapted and tested against the actual database engine and operator schema before production use.

### 4.53 Operator configuration

A configuration file is generated to document important processing parameters and expected input characteristics.

This is intended to make the analytical workflow easier to adapt and reproduce.

### 4.54 Basic tests and implementation checklist

The notebook includes basic tests and an operator implementation checklist.

The checklist covers areas such as:

- Input schema validation.
- Data quality.
- Processing configuration.
- Geographic aggregation.
- Suppression.
- Output generation.
- Performance considerations.
- Implementation review.

### 4.55 Performance reporting

The processing workflow records basic performance information to support an initial assessment of processing behaviour.

These measurements are limited to the synthetic dataset and the local execution environment.

They are not evidence of production performance at MNO scale.

### 4.56 Handover documentation

The notebook generates operator-oriented documentation and summaries intended to support implementation discussions.

The outputs include:

- Operator processing configuration.
- Data quality summaries.
- Processing performance reports.
- Privacy-related suppression summaries.
- Implementation checklist.
- Handover summary.

### 4.57 Implementation limitations

The notebook demonstrates an operator-oriented prototype, not a production MNO deployment.

Further work would be required to implement:

- Distributed processing.
- Production database integration.
- Operator-specific data connectors.
- Automated orchestration.
- Monitoring and alerting.
- Production access controls.
- Comprehensive integration and regression testing.
- Formal implementation acceptance criteria.

**Key outcome:** A prototype architecture demonstrating how MNO analytical workflows could be adapted for chunk-based processing, database aggregation and operator-side implementation.

---

## Notebook 08 — Privacy-Preserving MNO Analytics

**File:** `08_privacy_preserving_mno_analytics.ipynb`

### Objective

Notebook 08 explores privacy-by-design approaches for processing and analysing MNO mobility records.

MNO datasets may contain sensitive subscriber identifiers and detailed mobility information.

The notebook investigates methods for reducing the amount of identifiable information used during analysis and explores privacy-enhancing technologies that may support secure collaboration and aggregate analytics.

The objective is to demonstrate privacy-conscious analytical design rather than claim a production-ready privacy infrastructure.

### 4.58 Data minimisation

The notebook begins by selecting only the fields required for the analytical workflow.

The minimised dataset retains relevant fields such as:

- Subscriber identifier, temporarily required for pseudonymisation.
- Timestamp.
- Cell identifier.
- Latitude.
- Longitude.
- Record type, where required.

Unnecessary fields are excluded from the privacy-focused processing workflow.

### 4.59 HMAC-SHA256 pseudonymisation

The notebook demonstrates keyed pseudonymisation using HMAC-SHA256.

A subscriber identifier is transformed into a deterministic pseudonymous token using a secret key.

The implementation uses:

- Python's `hmac` module.
- SHA-256.
- A secret key stored outside the notebook.
- Environment-variable-based key loading.
- A `.env` file for local development.

The secret key is not intended to be committed to GitHub.

The resulting token can support consistent subscriber-level aggregation without retaining the original direct identifier in the working analytical sample.

**Security distinction:** HMAC pseudonymisation is not anonymisation. The key must remain confidential, and pseudonymous mobility records may still be sensitive or linkable.

### 4.60 Geographic aggregation

The notebook aggregates pseudonymised subscriber activity by geographic zone.

The objective is to reduce reliance on raw subscriber-level records when generating geographic analytical outputs.

The aggregation workflow includes:

- Geographic grid assignment.
- Subscriber-zone activity aggregation.
- Event counts.
- Nighttime activity.
- Weekend activity.
- Geographic subscriber counts.

### 4.61 Event contribution limits

The notebook demonstrates event contribution caps to limit the influence of highly active subscribers.

A configurable event cap is applied in the demonstration.

The purpose is to reduce disproportionate contributions from individual subscribers to aggregate geographic activity measures.

The contribution limits are methodological parameters and would need to be evaluated and documented for a real deployment.

### 4.62 Minimum-group suppression

Geographic groups below the configured minimum subscriber threshold are suppressed.

The demonstration uses a minimum group size of ten subscribers.

This reduces the risk of releasing highly granular outputs that may expose information about very small groups.

However, suppression alone does not guarantee anonymity and does not replace a formal privacy assessment.

### 4.63 Differential privacy demonstration

The notebook demonstrates the Laplace mechanism for adding noise to aggregate subscriber counts.

For a count query with sensitivity \(\Delta f\), the Laplace mechanism adds noise sampled from:

\[
\operatorname{Laplace}\left(0,\frac{\Delta f}{\epsilon}\right)
\]

Where:

- \(\epsilon\) is the privacy parameter.
- \(\Delta f\) is the query's sensitivity.
- Lower epsilon generally means stronger privacy and more noise.

For the demonstration, the query is the number of distinct subscribers in each individual geographic zone.

Each subscriber contributes at most one to that zone's count. Therefore, the sensitivity of an individual zone count is one under the add/remove-one-subscriber definition.

The notebook uses a configurable epsilon value and compares exact aggregate counts with noisy counts.

### 4.64 Differential privacy sensitivity analysis

Different epsilon values are examined to understand the privacy-utility trade-off.

The demonstration explores:

- 0.1
- 0.25
- 0.5
- 1
- 2
- 5

The objective is to show how stronger privacy settings generally introduce more noise into the released statistics.

**Important limitation:** The demonstration considers individual geographic count queries. A complete release involving multiple zones and multiple repeated queries requires appropriate privacy-budget accounting and composition analysis. The demonstration is not a complete formal differential privacy release system.

### 4.65 Private Set Intersection (PSI)

The notebook explores the concept of Private Set Intersection.

PSI allows two parties to identify elements present in both datasets without directly exchanging their entire underlying datasets.

The project includes:

- A PSI problem definition.
- A synthetic matching demonstration.
- A conceptual PSI implementation design.
- Implementation requirements for a secure protocol.

The HMAC-based matching example is a functional matching baseline, not a secure PSI protocol. In particular, sharing the same HMAC key between parties does not provide the security guarantees of a properly implemented PSI system.

### 4.66 Secure Multi-Party Computation (MPC)

The notebook introduces additive secret sharing as an educational demonstration of how a value can be split into multiple shares.

The individual shares do not independently reveal the original value under the assumptions of the demonstration.

The notebook then illustrates how aggregate computation can be reconstructed from shares.

This demonstrates the basic concept behind privacy-preserving computation across multiple parties.

However, the demonstration runs within a single Python process. It is not a production secure MPC protocol and does not establish distributed security between independently controlled organisations.

### 4.67 Homomorphic encryption

The notebook explores Paillier homomorphic encryption using the `phe` Python library.

The demonstration encrypts numerical values and performs an encrypted aggregation.

This illustrates how certain computations can be performed on encrypted values without first decrypting each individual input.

The implementation is educational and does not provide a complete production encryption architecture, key management system or security review.

### 4.68 Privacy technology comparison

The notebook compares several privacy-related approaches:

| Technique | Purpose | Project implementation |
|---|---|---|
| Data minimisation | Reduce unnecessary data collection and processing | Demonstrated |
| HMAC pseudonymisation | Replace direct identifiers with keyed tokens | Implemented |
| Geographic aggregation | Reduce individual-level data exposure | Demonstrated |
| Minimum-group suppression | Avoid releasing very small groups | Implemented |
| Differential privacy | Add calibrated noise to aggregate outputs | Demonstrated |
| Private Set Intersection | Support private set matching between parties | Conceptual design and non-secure matching baseline |
| Secure Multi-Party Computation | Support computation across multiple parties | Educational additive secret-sharing demonstration |
| Homomorphic encryption | Compute on encrypted values | Paillier encrypted-sum demonstration |

### 4.69 Privacy risk assessment

The notebook includes a privacy risk assessment and operator privacy checklist.

The assessment considers risks associated with:

- Direct subscriber identifiers.
- Pseudonymous subscriber records.
- Fine-grained geographic information.
- Small geographic groups.
- Secret-key exposure.
- Repeated aggregate releases.
- Data retention.
- Unauthorised access.
- Intermediate processing data.

### 4.70 Privacy-safe outputs

The notebook generates geographic aggregate outputs and privacy-related documentation.

Subscriber-level intermediate data and secret keys must not be published.

The generated outputs are subject to review before any public release.

### 4.71 Privacy limitations

The notebook does not constitute a formal privacy impact assessment or security audit.

Additional work would be needed for:

- Secure production PSI.
- Distributed MPC.
- Production key management.
- Formal differential privacy accounting.
- Privacy threat modelling.
- Access control and audit logging.
- Retention and deletion policies.
- Independent security review.
- Operator-specific legal and regulatory assessment.

**Key outcome:** A privacy-focused analytical prototype demonstrating data minimisation, keyed pseudonymisation, aggregation, differential privacy and introductory privacy-enhancing technology concepts.

---

# 5. Technical Architecture

The project follows a sequential analytical architecture.

```text
                    SYNTHETIC MNO DATA
                  CDR / IPDR RECORDS
                          |
                          v
          NOTEBOOK 01: DATA EXPLORATION
             AND QUALITY ASSESSMENT
                          |
                          v
          NOTEBOOK 02: RESIDENTIAL LOCATION
                    INFERENCE
                          |
                          v
          NOTEBOOK 03: RESIDENCE CLASSIFICATION
              AND GEOGRAPHIC PRESENCE
                          |
                          v
          NOTEBOOK 04: POPULATION DISPLACEMENT
                       DETECTION
                          |
                          v
          NOTEBOOK 05: VALIDATION AND
             METHODOLOGICAL ROBUSTNESS
                          |
                          v
          NOTEBOOK 06: POVERTY ESTIMATION
               AND SURVEY INTEGRATION
                          |
                          v
          NOTEBOOK 07: SCALABILITY AND
              OPERATOR IMPLEMENTATION
                          |
                          v
          NOTEBOOK 08: PRIVACY-PRESERVING
                   MNO ANALYTICS
                          |
                          v
             AGGREGATED ANALYTICAL OUTPUTS
             DOCUMENTATION AND HANDOVER
```

The notebooks are modular and address different stages of the analytical workflow. Some later notebooks reconstruct their required inputs from stored outputs or raw records to support independent execution.

---

# 6. Repository Structure

The intended repository structure is:

```text
mno-mobility-intelligence/
│
├── README.md
├── LICENSE
├── .gitignore
├── .env.example
├── requirements.txt
│
├── data/
│   ├── raw/
│   │   └── .gitkeep
│   │
│   ├── interim/
│   │   └── .gitkeep
│   │
│   ├── processed/
│   │   └── .gitkeep
│   │
│   └── synthetic/
│       └── .gitkeep
│
├── notebooks/
│   ├── 01_mno_data_exploration_and_quality_assessment.ipynb
│   ├── 02_home_location_inference.ipynb
│   ├── 03_residence_classification_and_geographic_presence.ipynb
│   ├── 04_population_displacement_detection.ipynb
│   ├── 05_validation_sensitivity_and_methodological_robustness.ipynb
│   ├── 06_poverty_estimation_and_survey_data_integration.ipynb
│   ├── 07_scalability_and_operator_implementation.ipynb
│   └── 08_privacy_preserving_mno_analytics.ipynb
│
├── src/
│   └── .gitkeep
│
├── sql/
│   └── .gitkeep
│
├── docs/
│   └── .gitkeep
│
├── reports/
│   └── .gitkeep
│
├── dashboard/
│   └── .gitkeep
│
└── tests/
    └── .gitkeep
```

The structure may evolve as notebook functionality is refactored into reusable Python modules, SQL scripts, formal tests and documentation.

Generated datasets, private intermediate files, database files and sensitive analytical outputs should remain outside the public repository.

---

# 7. Technology Stack

The project uses Python and its data science ecosystem for data processing, geospatial analysis, statistical modelling, database operations and privacy demonstrations.

| Category | Tools and libraries |
|---|---|
| Programming language | Python 3.11 |
| Data manipulation | Pandas, NumPy |
| Geospatial analysis | GeoPandas, Shapely |
| Spatial clustering | Scikit-learn, DBSCAN |
| Statistical analysis | SciPy, Scikit-learn |
| Visualisation | Matplotlib, Seaborn |
| Database | SQLite |
| SQL | SQL aggregation and query examples |
| Privacy | HMAC, SHA-256, differential privacy demonstration |
| Homomorphic encryption | `phe` |
| Environment management | Conda / Miniforge |
| Notebook environment | Jupyter Notebook |
| Version control | Git and GitHub |
| Configuration | JSON, environment variables, `.env` |
| Data processing | Chunk-based CSV processing |

---

# 8. Environment Setup

### 8.1 Clone the repository

```bash
git clone https://github.com/YOUR_USERNAME/mno-mobility-intelligence.git

cd mno-mobility-intelligence
```

Replace `YOUR_USERNAME` with the appropriate GitHub username.

### 8.2 Create a Conda environment

```bash
conda create -n mno-mobility python=3.11 -y

conda activate mno-mobility
```

### 8.3 Install dependencies

If a `requirements.txt` file is available:

```bash
python -m pip install -r requirements.txt
```

Alternatively, install the principal packages:

```bash
python -m pip install \
pandas \
numpy \
matplotlib \
seaborn \
scikit-learn \
scipy \
geopandas \
shapely \
jupyter \
python-dotenv \
phe
```

### 8.4 Launch Jupyter Notebook

```bash
jupyter notebook
```

Open the notebook directory and execute the notebooks in numerical order.

### 8.5 Data configuration

Place the synthetic source datasets in the appropriate local data directory.

Expected input files include:

```text
data/raw/MPD_sample_synthetic_ken_3000subs.csv

data/raw/MPD_sample_synthetic_ken_3000subs_full.csv
```

The notebooks should be configured to use the appropriate project root and input paths.

### 8.6 Privacy key configuration

Notebook 08 uses a secret key for HMAC-SHA256 pseudonymisation.

Create a local `.env` file in the project root:

```text
MNO_PSEUDONYM_SECRET=YOUR_OWN_RANDOM_SECRET_KEY
```

Load it through `python-dotenv`:

```python
from dotenv import load_dotenv
import os

load_dotenv(PROJECT_ROOT / ".env")

secret_key = os.getenv("MNO_PSEUDONYM_SECRET")
```

Generate a secure random key locally rather than using a hardcoded example.

The `.env` file must be excluded from Git tracking.

Never commit actual secret keys, credentials or environment-specific security information to GitHub.

---

# 9. Key Methodological Principles

The project follows several important principles to reduce analytical errors and improve interpretability.

### 9.1 Separate observed data from simulator-generated labels

Simulator-generated labels must not be used as inference features.

Where available, they may be used only for clearly identified synthetic reference evaluation.

### 9.2 Distinguish residence from presence

A subscriber generating activity in a geographic area does not automatically establish that the subscriber resides there.

Residence inference requires recurring evidence over a defined observation period.

### 9.3 Treat insufficient evidence separately from absence

A subscriber with insufficient observations should not automatically be classified as absent.

The methodology preserves uncertainty rather than forcing every subscriber into a binary residence or non-residence classification.

### 9.4 Account for sparse and irregular observations

Network activity is not continuous GPS tracking.

The methodology must account for differences in subscriber activity, event frequency, nighttime coverage and geographic observation density.

### 9.5 Avoid look-ahead leakage

Baseline observations must be separated from follow-up observations during temporal validation.

Future information must not be used to construct historical residential estimates.

### 9.6 Validate parameter choices

Spatial clustering, nighttime windows, observation thresholds and movement distances influence analytical outputs.

The project uses sensitivity analysis to understand how these decisions affect inference.

### 9.7 Avoid overstating confidence

Rule-based evidence scores are not automatically calibrated probabilities.

Method agreement and temporal stability are useful diagnostics, but they do not prove that an inferred residence is correct.

### 9.8 Aggregate before sharing

Where possible, geographic analytical outputs should be generated from minimised and aggregated data.

Subscriber-level mobility information must be treated as sensitive.

---

# 10. Data Protection and Confidentiality

MNO mobility data can reveal sensitive information about individuals, including recurring locations and movement patterns.

The project therefore incorporates privacy considerations throughout its design.

Key practices include:

- Data minimisation.
- Keyed pseudonymisation.
- Secret management outside notebooks.
- Geographic aggregation.
- Minimum-group suppression.
- Differential privacy demonstrations.
- Exploration of PSI, MPC and homomorphic encryption.
- Review of privacy risks associated with intermediate outputs.

### Repository protection

The following categories of files should not be committed to a public GitHub repository:

```gitignore
# Raw and processed data
data/raw/
data/interim/
data/processed/
data/synthetic/*.csv
data/synthetic/*.parquet

# Local secrets
.env
.env.*

# Databases and local storage
*.db
*.sqlite
*.sqlite3

# Python environments
.venv/
venv/
__pycache__/

# Jupyter checkpoints
.ipynb_checkpoints/

# Local system files
.DS_Store

# Generated analytical outputs
reports/*.csv
reports/*.parquet
```

If a secret or sensitive dataset has already been committed, adding it to `.gitignore` does not remove the existing Git history. The exposure must be addressed separately, including secret rotation where appropriate.

---

# 11. Key Outputs and Deliverables

The eight notebooks generate analytical outputs and intermediate artefacts that support subsequent stages of the project.

| Notebook | Main outputs |
|---|---|
| 01 | Data quality summaries, subscriber activity analysis, temporal and geographic diagnostics |
| 02 | Home location estimates, subscriber classifications, nighttime candidates, cell coordinate validation |
| 03 | Residence classifications, geographic presence summaries and administrative area assignments |
| 04 | Baseline/follow-up movement analysis, potential relocation indicators and geographic movement summaries |
| 05 | Temporal holdout results, parameter sensitivity results, method comparisons and robustness diagnostics |
| 06 | Geographic MNO features, synthetic survey data, integrated zone-level dataset, poverty modelling and evaluation results |
| 07 | SQLite aggregation database, processing reports, operator configuration, quality reports and handover documentation |
| 08 | Privacy-safe geographic aggregates, differential privacy demonstrations, PSI/MPC design materials, privacy risk assessment and implementation checklist |

These outputs are generated locally and should be reviewed before any public release.

Not all intermediate outputs are suitable for public sharing, particularly those containing subscriber-level records or sensitive geographic information.

---

# 12. Project Limitations

This project is a methodological research and implementation prototype based on synthetic MNO data.

It is not a deployed MNO analytics system and does not claim validated performance on real subscriber records.

The most important limitations are outlined below.

### 12.1 Synthetic data

The dataset is simulated and does not reproduce every characteristic of real operator data.

Actual MNO records may differ in:

- Network event frequency.
- Cell-site density.
- Subscriber activity.
- Roaming behaviour.
- Network architecture.
- Cell handover behaviour.
- Timestamp conventions.
- Geographic precision.
- Data availability and completeness.

### 12.2 Limited observation period

The dataset covers 28 days.

This limits the ability to evaluate longer-term residential stability, seasonal migration, persistent displacement and changes in subscriber behaviour over extended periods.

### 12.3 Lack of independent ground truth

Simulator-generated reference labels are not equivalent to independently verified residential locations.

Temporal stability and agreement between inference methods are useful diagnostics, but do not establish real-world residential accuracy.

### 12.4 Cell-site location limitations

A cell site's coordinates do not necessarily represent the precise geographic location of a subscriber when an event occurs.

Real-world analysis may require additional network information, including sector identifiers, azimuth, coverage characteristics and network topology.

### 12.5 Poverty estimation limitations

The household survey and poverty labels used in Notebook 06 are synthetic.

Consequently, the model results cannot be interpreted as evidence that MNO activity predicts actual household poverty.

### 12.6 Scalability limitations

Notebook 07 demonstrates chunk-based processing and local SQLite aggregation.

It does not establish performance under production operator workloads or distributed processing environments.

### 12.7 Privacy limitations

Notebook 08 demonstrates selected privacy techniques but does not implement a complete secure multi-party analytics platform.

HMAC pseudonymisation, suppression, differential privacy, PSI, MPC and homomorphic encryption have different security properties and should not be treated as interchangeable.

Formal privacy accounting, secure protocol implementation, threat modelling and independent security review would be necessary before real-world deployment.

---

# 13. Future Development Roadmap

The current project establishes an analytical foundation. Further development would focus on improving its methodological robustness, operational readiness and ability to adapt to different MNO environments.

### 13.1 Reusable analytical method library

Refactor notebook logic into reusable Python modules covering:

- Data validation.
- Temporal filtering.
- Geographic processing.
- Residential candidate generation.
- Spatial clustering.
- Residence classification.
- Displacement detection.
- Geographic aggregation.
- Validation and sensitivity analysis.

Introduce versioned configuration files to make parameter changes traceable and reproducible.

### 13.2 Multi-operator schema adaptation

Develop additional schema adapters to simulate different MNO data environments.

Evaluate how different event structures, timestamp formats, cell identifiers and geographic precision affect inference.

### 13.3 More extensive testing

Expand the existing testing approach to include:

- Unit tests.
- Integration tests.
- Regression tests.
- Edge-case tests.
- SQL correctness checks.
- Temporal leakage checks.
- Geographic calculation tests.
- Data quality acceptance tests.

### 13.4 Operator implementation specifications

Develop formal technical specifications containing:

- Input and output schemas.
- Data processing requirements.
- Methodology and pseudocode.
- Parameter definitions.
- SQL examples.
- Validation criteria.
- Error handling.
- Privacy requirements.
- Performance expectations.

### 13.5 Improved displacement analysis

Extend the displacement workflow with longer baseline periods, multiple follow-up periods, seasonal movement considerations and external event timelines.

### 13.6 Poverty estimation with real survey data

Explore suitable publicly available household survey data, subject to licence and access conditions.

Evaluate the integration of MNO geographic features with properly weighted survey outcomes and independently measured socioeconomic indicators.

### 13.7 Production-oriented processing

Explore distributed processing technologies and database integrations that may be appropriate for larger MNO datasets.

Potential areas include:

- PySpark.
- PostgreSQL/PostGIS.
- Cloud-based batch processing.
- Automated workflow orchestration.
- Processing monitoring.
- Data quality observability.

### 13.8 Advanced privacy engineering

Further develop the privacy component through:

- Formal differential privacy accounting.
- Secure PSI implementations.
- Distributed MPC protocols.
- Production-grade homomorphic encryption workflows.
- Privacy threat modelling.
- Key lifecycle management.
- Access control and audit logging.
- Data retention and deletion policies.

### 13.9 Technical and non-technical training

Develop training materials to explain:

- How CDR and IPDR data work.
- How residence inference is performed.
- How geographic presence is interpreted.
- How displacement signals should be evaluated.
- What the data cannot reliably establish.
- Why uncertainty and privacy matter.

---

# 14. Relevance to Humanitarian and Development Applications

The project explores analytical methods that could potentially support humanitarian and development programmes.

### Geographic targeting

Residential location inference may help estimate whether subscribers are associated with a defined geographic target area over a specified observation period.

### Emergency response

Changes in mobility patterns may provide additional evidence for investigating population movement following an emergency.

### Population mobility monitoring

Aggregated mobility information may help describe movement patterns between geographic areas, subject to data quality, privacy and interpretation constraints.

### Poverty research

Geographic MNO activity features may be explored alongside independently collected household survey information to investigate potential relationships with socioeconomic conditions.

### Evidence-based intervention design

Understanding the strengths and limitations of MNO data can help programme teams determine which questions can reasonably be answered using mobility records.

These are potential applications, not demonstrated real-world programme outcomes from this synthetic-data project.

---

# 15. Current Project Completion Status

| Component | Status |
|---|---|
| MNO data exploration and quality assessment | Completed prototype |
| Residential location inference | Implemented |
| Residence classification | Implemented |
| Geographic presence estimation | Implemented |
| Population displacement detection | Exploratory implementation |
| Temporal validation | Implemented |
| Sensitivity analysis | Implemented |
| Poverty estimation and survey integration | Synthetic demonstration |
| Chunk-based processing | Implemented prototype |
| Operator schema configuration | Demonstrated |
| SQLite aggregation | Implemented |
| Operator implementation documentation | Initial prototype |
| HMAC pseudonymisation | Implemented |
| Geographic suppression | Implemented |
| Differential privacy | Demonstration |
| PSI | Conceptual design and non-secure matching baseline |
| MPC | Educational demonstration |
| Homomorphic encryption | Educational demonstration |
| Production MNO deployment | Not implemented |
| Independent real-world validation | Not performed |
| Formal security review | Not performed |

---

# 16. Conclusion

The MNO Mobility Intelligence Project explores how mobile network records can be transformed into useful geographic and population-level insights through spatio-temporal analysis, statistical inference, geospatial processing and privacy-conscious data engineering.

Across eight notebooks, the project progresses from understanding synthetic MNO records to inferring residential locations, classifying residence evidence, exploring potential population displacement, evaluating methodological robustness, integrating geographic activity with synthetic household survey data, designing operator-oriented processing workflows and demonstrating privacy-enhancing technologies.

An important focus throughout the project is recognising the limitations of mobility data.

Network activity is not continuous location tracking. A cell-site observation does not necessarily identify a subscriber's precise location. Missing observations do not establish absence. A change in inferred location does not prove forced displacement. Geographic activity does not directly measure poverty.

The methodology therefore places emphasis on evidence strength, uncertainty, temporal validation, sensitivity analysis, data minimisation and careful interpretation.

The project is intended as a foundation for developing repeatable analytical methods that could be adapted and evaluated with real MNO datasets in collaboration with operators and relevant technical, privacy and programme stakeholders.

**The ultimate objective is not simply to build a model that predicts where subscribers live, but to develop analytical methods that are explainable, testable, adaptable, privacy-conscious and practical to implement in real-world MNO environments.**

---

## Disclaimer

This is an independent research and portfolio project developed using synthetic MNO mobility data.

It is not affiliated with, commissioned by or endorsed by any mobile network operator or humanitarian organisation.

All subscriber identifiers and mobility observations are synthetic. No real subscriber-level mobility records are represented.

The results are methodological demonstrations and should not be used for operational targeting, beneficiary eligibility decisions, real-world poverty measurement or confirmed displacement classification without appropriate real-world validation, privacy review and operational testing.
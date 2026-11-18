
# MNO Mobility Intelligence: Residential Inference and Population Displacement Detection

## Overview

This project develops a reproducible analytical framework for extracting
residential location patterns and population mobility indicators from
Mobile Network Operator (MNO) data.

The objective is to investigate how mobile network records, including
Call Detail Records (CDR) and Internet Protocol Detail Records (IPDR),
can support:

- Residential location inference.
- Residence stability classification.
- Geographic presence estimation.
- Population movement and displacement detection.
- Administrative-area population aggregation.

The project is designed around practical MNO implementation constraints,
including sparse observations, imperfect cell-site coordinates,
uncertain residential patterns, limited ground truth and data privacy.

## Motivation

This project explores analytical methodologies relevant to humanitarian
response, population monitoring and evidence-based programme targeting.

It is particularly motivated by the application of spatio-temporal
modelling and privacy-conscious analytics to humanitarian operations.

## Project Status

| Component | Status |
|---|---|
| Data exploration and quality assessment | Completed |
| Home location inference | Completed |
| Residence classification | Completed |
| Population displacement detection | Planned |
| Method validation and sensitivity analysis | Planned |
| Privacy-preserving implementation | Planned |
| Operationalisation and technical specifications | Planned |

## Methodology

### 1. Data Exploration

- Subscriber activity analysis.
- Temporal activity patterns.
- Cell-site coordinate consistency.
- Geographic coverage assessment.
- Data quality and representativeness limitations.

### 2. Home Location Inference

- Nighttime activity analysis.
- Distinct-night presence features.
- Frequency-based candidate selection.
- DBSCAN spatial clustering.
- Weighted nighttime scoring.
- Haversine distance calculations.

### 3. Residence Classification

- Residential stability indicators.
- Nighttime home consistency.
- Recurring alternative nighttime locations.
- Evidence-level classification.
- Geographic assignment to administrative areas.

### 4. Population Displacement Detection

Planned work will examine persistent changes in inferred residential
patterns, origin-destination movements and geographic inflows/outflows.

### 5. Validation and Operationalisation

Planned work will investigate parameter sensitivity, temporal validation,
scalability, implementation specifications and privacy-preserving
aggregation.

## Technology Stack

- Python
- Pandas and NumPy
- Scikit-learn
- GeoPandas
- Shapely
- SciPy
- Matplotlib
- Jupyter Notebook

## Data and Privacy

The project uses synthetic MNO-like data for methodological development.

No real subscriber records, personal identifiers, individual mobility
trajectories or individual inferred residential coordinates are published.

Public geographic boundary data is documented separately with source
and licence information.

## Important Limitations

- Synthetic data does not establish real-world model performance.
- Network-observed presence does not necessarily establish residence.
- Inferred residential locations are subject to spatial uncertainty.
- Subscriber counts are not equivalent to total population estimates.
- Observed movement does not independently establish forced displacement.
- Classification thresholds require further sensitivity testing and validation.

## Repository Structure

See the notebooks, source code and documentation directories for
implementation details.

## Disclaimer

This is an independent research and portfolio project. It is not
affiliated with any mobile network operator.

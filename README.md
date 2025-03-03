# PFAS Contamination Forecasting

This project uses machine learning to forecast Per- and Polyfluoroalkyl Substances (PFAS) contamination levels across different regions of the United States. The model analyzes historical water quality data from the EPA's Unregulated Contaminant Monitoring Rule (UCMR) program to predict future PFAS concentrations.

## Table of Contents
- [Project Overview](#project-overview)
- [Background on PFAS](#background-on-pfas)
- [Dataset](#dataset)
- [Methodology](#methodology)
  - [Data Preprocessing](#data-preprocessing)
  - [Feature Engineering](#feature-engineering)
  - [Model Selection](#model-selection)
- [Results](#results)
  - [Model Performance](#model-performance)
  - [Regional Predictions](#regional-predictions)
- [Limitations and Future Work](#limitations-and-future-work)
- [How to Use This Repository](#how-to-use-this-repository)
- [Requirements](#requirements)

## Project Overview

The goal of this project is to train a model to forecast future levels of PFAS contamination in different U.S. regions using historical EPA water quality data. The model leverages data from 2001 to 2023 to forecast PFAS levels for 2024 and beyond. This forecasting capability can help authorities:

- Plan for future contamination risks
- Implement protective measures
- Inform the public about potential exposure hazards

## Background on PFAS

PFAS (Per- and Polyfluoroalkyl Substances) are harmful man-made chemicals that are highly persistent in the environment and human body. They are found in various consumer products, industrial applications, and firefighting foams. Due to their chemical stability, PFAS are often referred to as "forever chemicals" and can accumulate in the environment and human body over time.

Health effects associated with PFAS exposure include:
- Increased cholesterol levels
- Changes in liver enzymes
- Decreased vaccine response in children
- Increased risk of certain cancers
- Developmental effects in infants

## Dataset

The data comes from the EPA's Unregulated Contaminant Monitoring Rule (UCMR) program, which collects water samples from across the United States. The dataset includes:

- Water samples collected from 2001 to 2024
- Public Water System (PWS) information
- Reading levels for various PFAS contaminants
- Water system type (e.g., groundwater, surface water)
- Geolocation data for each public water system
- Additional data on water treatment methods

The primary focus is on UCMR5 data, as UCMR3 (the only other UCMR with PFAS readings) occurred 8 years prior to UCMR5, making temporal analysis challenging.

## Methodology

### Data Preprocessing

- Excluded Lithium readings (non-PFAS contaminant)
- Set readings below Minimum Reporting Level (MRL) to 0
- Added missing records through forward/backward filling methods
- Applied log transformation to address skewed distributions in contaminant levels
- Created a cohesive time series for analysis

### Feature Engineering

- Created temporal features (month, season)
- Added lagged features (prior analytical result values)
- Performed one-hot encoding for categorical variables:
  - Contaminant types
  - Facility water types
  - Method IDs
  - EPA Regions
  - Disinfectant types
  - Treatment information
- Split data into training (2023) and testing (2024) sets

### Model Selection

The project uses XGBoost (eXtreme Gradient Boosting), a gradient-boosted decision tree algorithm that offers several advantages for this type of data:

- Ability to handle missing data effectively
- Robust performance with imbalanced data
- Scalability for large datasets
- Feature importance scoring

The implementation includes:
- Sample weights to handle class imbalances
- Feature selection using a threshold of 0.02 importance
- Optimized hyperparameters including learning rate, tree depth, and regularization
- Cross-validation to ensure model robustness

## Results

### Model Performance

The model achieves strong performance metrics:

| Metric | Training Set | Testing Set |
|--------|-------------|------------|
| MSE    | 3.955e-07   | 4.086e-07  |
| RMSE   | 6.289e-04   | 6.393e-04  |
| MAE    | 2.035e-05   | 2.030e-05  |
| R²     | 0.940 (94.0%) | 0.934 (93.4%) |
| Pearson R | 0.972    | 0.968      |

The similar performance between training and testing sets indicates the model is not overfitting and generalizes well to new data.

### Regional Predictions

Key findings from the regional analysis:

**Highest Predicted PFAS Levels:**
- **Region 9**: Shows highest predicted concentrations, driven by elevated levels in Guam and Northern Mariana Islands.
- **Region 5**: Second highest predicted levels, with Minnesota contributing significantly.
- **Region 3**: Third highest predicted levels, with Delaware showing elevated concentrations.

**Lowest Predicted PFAS Levels:**
- **Region 8**: Consistently lower predictions across all states
- **Region 10**: Lower than average predictions 
- **Region 2**: Relatively stable, lower concentrations

## Limitations and Future Work

**Limitations:**
- Public Water Sources' (PWS) coordinates are difficult to extract from EPA geographical data
- Limited historical PFAS measurement data
- Irregular time series with non-constant spacing of observations
- Left-censored data (measurements below detection limits)

**Future Research Directions:**
- Examine correlations between non-PFAS contaminants and PFAS results
- Predict PFAS levels for more granular locations (city or county level)
- Refine the model with new data as more PFAS readings become available
- Enhance geographical analysis by improving PWS coordinate data integration

## Acknowledgments

Data for this project was obtained from the Environmental Protection Agency's (EPA) Unregulated Contaminant Monitoring Rule (UCMR) program.

# Estimating Field-Level Irrigation Water Use Using Sentinel-1

This repository contains the R workflow used to develop the modelling framework presented in:

**“Estimating Continuous Field-Level Irrigation Water Use in Australian Cotton Using Sentinel-1 Observations”**

The framework uses Sentinel-1 Synthetic Aperture Radar (SAR) observations, rainfall data, and field-level irrigation records to detect irrigation events and estimate irrigation water application at the field level.

## Purpose

The code is provided to:

1. Document the modelling workflow used in the study.
2. Improve transparency and reproducibility of the modelling methodology.
3. Provide a general framework that can be adapted to other datasets, study areas, and applications.

The supplied R Markdown file explains the major processing and modelling steps, including the expected inputs and outputs of the main functions.

> **Note:** The code is intended as a general modelling template rather than a directly executable reproduction of the study. Users will need to adapt data paths, field identifiers, variable names, and potentially model parameters to their own datasets.

## Modelling Workflow

The general workflow implemented in the R Markdown consists of:

1. **Field selection and data preparation**

   * Identification of irrigated and non-irrigated fields.
   * Selection of a non-irrigated control field.
   * Optional partitioning of fields into training and independent testing subsets.
   * Reshaping Sentinel-1 and irrigation observations into the required format.

2. **Design matrix creation**

   * Combination of field-level irrigation records and Sentinel-1 observations.
   * Addition of control-field Sentinel-1 observations.
   * Integration of rainfall and other climate information.
   * Creation of additional variables required for modelling.

3. **Irrigation-volume modelling**

   * A Generalised Additive Model (GAM) is used to represent the non-linear relationship between irrigation volume, Sentinel-1 backscatter, seasonality, and rainfall.
   * A tensor-product smooth represents the seasonally varying relationship between Sentinel-1 observations from the target and control fields.
   * A separate smooth term accounts for the influence of rainfall.
   * A Tweedie response distribution is used to accommodate the combination of zero irrigation observations and positive continuous irrigation volumes.

4. **Model evaluation and application**

   * The workflow can be used to separate fields into training and testing datasets for evaluating spatial generalisation.
   * Alternatively, all available fields can be retained for model fitting or subsequent application.

## Data Requirements

The workflow requires three main types of input data.

### Sentinel-1 observations

A wide-format dataset containing:

* observation date;
* field-specific Sentinel-1 backscatter observations; and
* observations for suitable non-irrigated fields that can potentially be used as controls.

### Irrigation records

Field-level irrigation records containing:

* irrigation date or observation interval;
* field identifiers; and
* irrigation water application volumes.

Fields without corresponding irrigation records can be treated as non-irrigated fields within the supplied workflow.

### Climate data

Daily climate information corresponding to the study period. The model presented in the paper uses rainfall as an explanatory variable.

The original study used Australian SILO climate data, but equivalent climate datasets may be substituted provided that the required variables and temporal resolution are available.

## Control Field

An important component of the framework is the use of a **non-irrigated control field**.

The control field provides a reference Sentinel-1 response to environmental changes, particularly rainfall, that are unrelated to irrigation. Comparing the Sentinel-1 response of cropped fields with this control assists the model in distinguishing irrigation-related changes from rainfall-induced changes in backscatter.

Users applying the workflow to other study areas should therefore identify an appropriate non-irrigated control field or adapt the control-field selection procedure to their application.

## Software

The analysis is implemented in **R** and makes use of packages including:

* `mgcv`
* `dplyr`
* `tidyr`
* `lubridate`

Additional packages may be required for other processing, analysis, and visualisation steps included in the R Markdown file.

## Data Availability

The field-level irrigation and operational data used in the original study contain commercially sensitive information and cannot be made publicly available.

Consequently, this repository provides the modelling methodology and code structure rather than the original study dataset. Users wishing to run the workflow will need to provide their own data in the required format and adapt the relevant sections of the R Markdown accordingly.

## Reproducibility and Adaptation

The code reflects the modelling framework developed for the study and has been generalised where possible to facilitate application to other datasets.

However, users should carefully review and adapt:

* field naming conventions;
* input data structure;
* control-field selection;
* climate variables;
* seasonal variables;
* model basis dimensions and smoothing parameters; and
* training and validation strategies.

**Model settings used in the original study should not necessarily be assumed to be optimal for other crops, regions, seasons, or irrigation systems.**

## Citation

If you use or adapt this workflow in your research, please cite the associated paper:

> *Estimating Continuous Field Level Irrigation Water Use in Australian Cotton Using Sentinel-1 Observations.*

Full citation details will be added following publication.

## Disclaimer

This code is provided for research and reproducibility purposes. Users are responsible for validating the methodology and model performance for their specific datasets and applications.

# EV SmartHub — AI Analytics for EV Charging & Battery Management

AI-powered decision-support platform for EV charging operations, combining data quality diagnostics, physics-informed feature engineering, machine learning benchmarking and an interactive Gradio interface.

## Project Overview

EV SmartHub is an AI analytics prototype designed to support Charge Point Operators (CPOs), fleet managers and other EV ecosystem stakeholders.

The system addresses four operational decision areas:

- Charging wait-time prediction
- Grid load forecasting
- Dynamic price optimisation
- Battery Remaining Useful Life (RUL) estimation

Rather than developing a single prediction model, the project combines multiple analytical services into one interactive decision-support platform.

## Business Problem

Modern EV charging infrastructure often operates with limited visibility into:

- battery degradation
- charging duration
- future electricity demand
- appropriate session pricing
- maintenance requirements

This creates several business risks:

1. Inefficient charging and increased battery degradation
2. Pricing that does not reflect energy demand or battery wear
3. Peak-load clustering and grid instability
4. Reactive rather than predictive maintenance
5. Poor customer experience caused by uncertain charging times

EV SmartHub was designed to transform operational and battery telemetry into actionable decisions.

## Solution Architecture

The solution consists of three main layers:

### 1. Physics-Informed Data Engineering

Raw operational data is cleaned, validated and transformed into domain-aligned features and targets.

### 2. AutoML-Style Model Benchmarking

Multiple regression algorithms are trained and compared for each business task.

### 3. Decision-Service Layer

The best-performing models are exposed through a multi-tab Gradio application for non-technical business users.

## Four Analytical Services

### Wait-Time Prediction

**Business question:**  
How long will a charging session take?

**Target:** Charging Duration

Key inputs include:

- Battery capacity
- Starting SOC
- Target SOC
- Charging rate
- Energy required

The engineered relationship follows the physical principle:

`Charging Duration ≈ Energy Required / Charging Power`

---

### Load Forecasting

**Business question:**  
How much charging demand should the station expect at a specific time?

**Target:** Energy Consumed (kWh)

Key inputs:

- Hour
- Day of week

The engineered demand profile reflects realistic daily charging behaviour:

- low overnight demand
- moderate daytime demand
- evening peak demand

---

### Price Optimisation

**Business question:**  
What is an appropriate charging-session price?

**Target:** Charging Cost

The pricing logic combines:

- Energy consumption
- Base energy rate
- Time-of-day effects
- Peak-period price adjustments

This allows the system to support transparent time-of-use pricing scenarios.

---

### Battery Maintenance / RUL

**Business question:**  
How much useful battery life remains?

**Target:** Remaining Useful Life (RUL)

Main inputs:

- Charge cycles
- Battery temperature

RUL was engineered using a degradation relationship where additional charging cycles and elevated temperature reduce remaining battery life.

## Data Quality Challenge

One of the most important findings occurred before model development.

Initial models trained directly on the raw target variables produced **negative R² values**, meaning that the models performed worse than simply predicting the mean.

Exploratory analysis showed that several target variables lacked realistic relationships with their expected drivers.

Examples included:

- charging cost showing little relationship with energy consumed
- charging duration showing weak relationship with charging rate
- RUL showing inconsistent relationships with battery stress indicators

The maintenance dataset also contained extreme outlier behaviour.

Rather than hiding this issue or continuing to train models on unreliable labels, the project redesigned the targets using explicit physical and economic assumptions.

## Physics-Informed Target Engineering

The project therefore moved from:

> "Predict what the historical CSV says"

to:

> "Model what should happen according to transparent domain rules."

Examples include:

### Energy Demand

Energy demand was engineered using a time-of-day charging profile.

### Charging Cost

`Charging Cost = Energy × Base Rate × Time-of-Day Factor + Noise`

### Charging Duration

`Duration ≈ Energy / Charging Rate`

### Remaining Useful Life

`RUL ≈ Maximum Life − Cycle Wear − Temperature Wear`

This transformed the project from a noisy historical prediction task into a simulation-based decision-support prototype.

## Tools & Technologies

- Python
- Pandas
- NumPy
- Scikit-learn
- Matplotlib
- Seaborn
- Plotly
- Gradio
- Machine Learning
- Feature Engineering
- AutoML-style Benchmarking
- Regression Analysis
- Data Visualization

## Machine Learning Models

For each analytical service, several algorithms were benchmarked:

- Linear Regression
- Decision Tree Regressor
- Random Forest
- Tuned Random Forest
- Gradient Boosting
- Support Vector Regression (SVR)
- Multi-Layer Perceptron (MLP)

Models were evaluated using:

- R²
- Mean Absolute Error (MAE)

An 80/20 train-test split was used, and the model with the strongest R² was selected for each task.

## Model Benchmarking Results

After physics-informed target engineering:

- R² values changed from negative to clearly positive across all four analytical tasks.
- Tree-based models generally outperformed simpler linear baselines.
- Model rankings remained relatively stable across repeated runs.
- The strongest-performing model was selected independently for each business service.

This AutoML-style comparison avoided relying on a single algorithm for every analytical problem.

## Exploratory Data Analysis

The redesigned data produced interpretable relationships consistent with EV operations.

Key observations included:

- Energy demand increasing during peak charging hours
- Strong relationship between energy consumption and charging cost
- Charging duration decreasing as charger power increases
- RUL decreasing as battery cycles and temperature increase
- Structured time-of-day effects in grid demand

## Interactive Gradio Application

EV SmartHub was deployed as a multi-tab interactive application.

The interface includes:

1. Data Setup
2. EDA Dashboard
3. AutoML Comparison
4. Wait-Time Prediction
5. Load Forecasting
6. Price Optimiser
7. Battery Maintenance / RUL

Business users interact with sliders and dropdowns rather than the underlying Python models.

## Business Outputs

### Wait-Time Prediction

Returns estimated charging duration based on battery and charger characteristics.

### Load Forecasting

Predicts expected charging demand for selected time periods.

### Price Optimiser

Returns predicted session cost and implied price per kWh.

### Maintenance

Estimates remaining battery life and provides a health indicator such as:

- Healthy
- Maintenance Needed

## Business Value

EV SmartHub demonstrates how analytics can support:

- charging capacity planning
- dynamic pricing
- predictive maintenance
- battery asset management
- grid-demand management
- customer wait-time estimation
- data-driven charging operations

The project also demonstrates how machine learning can be translated into a user-facing analytical product rather than remaining only in a notebook.

## Key Analytical Lesson

A major insight from this project was that **better algorithms cannot compensate for unreliable target data**.

The initial negative R² results demonstrated that data validation and problem formulation must come before model optimisation.

Instead of forcing increasingly complex models onto weak labels, the project redesigned the analytical problem using transparent physics-informed assumptions.

## Limitations

This project is a prototype and has several important limitations:

- Several target variables were engineered rather than observed from reliable real-world operational data.
- The resulting relationships may appear cleaner than those observed in production environments.
- Battery chemistry differences are not fully modelled.
- Weather, traffic and other real-time external factors are not included.
- Additional validation using real charging-station telemetry would be required before production deployment.

## Ethical Considerations

Continuous monitoring of EV charging behaviour could create privacy risks.

Future implementations should minimise personally identifiable movement data and apply appropriate data masking and governance controls.

## Future Improvements

Potential extensions include:

- validation using live charging-station telemetry
- battery-chemistry-specific degradation models
- weather and traffic integration
- real-time grid data
- time-series demand forecasting
- cloud deployment
- model monitoring
- explainable AI

## Repository Structure

```text
ev-smarthub-ai-analytics/
│
├── README.md
│
├── notebooks/
│   └── ev_smarthub_analysis.ipynb
│
├── app/
│   └── ev_smarthub_app.py
│
├── report/
│   └── EV_SmartHub_AI_Analytics_Report.pdf
│
└── images/
    ├── architecture.png
    ├── eda_dashboard.png
    ├── automl_comparison.png
    └── gradio_interface.png

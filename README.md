DMBI Experiment 10 – Bank Customer Churn System
Experiment title
Air Quality Analysis and Carbon Monoxide (CO) Concentration Prediction Using Data Mining Techniques

The requested project heading Bank Customer Churn System is used in the filenames and document title. However, the uploaded Experiment 10 PDF is an air-quality prediction practical, not a bank customer churn practical. It predicts continuous Carbon Monoxide concentration (CO(GT)) using regression models. It does not use a bank churn dataset or perform churn classification. The implementation below preserves the actual experiment from the supplied PDF rather than silently changing its subject.
Aim
To analyze historical air-quality multisensor data and implement regression-based data mining models—Multivariate Linear Regression and Decision Tree Regression—to estimate Carbon Monoxide (CO) concentration, incorporating sensor dynamics, exploratory data analysis and a chronological train-test pipeline.
Dataset
- Dataset: UCI Air Quality Multisensor Array Benchmark Dataset.
- Source URL: https://archive.ics.uci.edu/static/public/360/air+quality.zip
- Original observations: 9,471 rows as loaded from the archive.
- After removing blank rows: 9,357 records.
- After target validation: 7,674 records.
- Target: CO(GT) in mg/m³.
- Predictors: PT08.S1(CO), PT08.S2(NMHC), PT08.S3(NOx), PT08.S4(NO2), PT08.S5(O3), T, RH, and AH.
Other reference gas concentrations (NMHC(GT), C6H6(GT), NOx(GT), and NO2(GT)) are excluded from the predictor set to avoid target leakage, as explained in the source PDF.
Theory
1. MOS chemoresistive sensor dynamics
Metal-Oxide Semiconductor sensors respond to gas concentrations through surface oxygen ionosorption and catalytic pollutant oxidation. Temperature and humidity can affect sensor response, so multiple sensor readings and weather measurements are used together for software calibration.
2. CRISP-DM and zero-leakage pipeline
The experiment follows the CRISP-DM approach. The dataset is cleaned, sorted by time, and split chronologically. Random shuffling is avoided because future environmental patterns must not leak into training data.
3. Descriptive statistics
The source PDF reports the following statistics for the 7,674 validated CO observations:
Statistic	CO(GT)
Count	7,674
Mean	2.153 mg/m³
Standard deviation	1.453 mg/m³
Minimum	0.100 mg/m³
Q1	1.100 mg/m³
Median	1.800 mg/m³
Q3	2.900 mg/m³
Maximum	11.900 mg/m³


4. Correlation analysis
Pearson correlation values reported in the PDF:
Predictor	Correlation with CO(GT)
PT08.S2(NMHC)	+0.916
PT08.S1(CO)	+0.879
PT08.S5(O3)	+0.854
PT08.S4(NO2)	+0.631
RH	+0.049
AH	+0.049
T	+0.022
PT08.S3(NOx)	-0.703


5. Regression algorithms
- Multivariate Linear Regression: Fits a linear relationship between the eight predictors and the continuous CO target.
- Decision Tree Regression: Recursively splits the feature space to model non-linear relationships.
6. Evaluation metrics
- MAE: Mean Absolute Error; average absolute prediction error in mg/m³.
- RMSE: Root Mean Squared Error; penalizes larger errors more strongly.
- R²: Coefficient of determination; proportion of target variance explained by the model.
Implementation steps
1. Import required libraries.
2. Download and read the official UCI ZIP dataset.
3. Remove empty columns and rows.
4. Replace sensor failure sentinel values (-200) with NaN.
5. Combine Date and Time into DateTime and sort chronologically.
6. Remove rows with missing CO(GT) target values.
7. Select the eight permitted predictor features.
8. Perform EDA: descriptive statistics, histogram, box plot, daily trend, rolling average and monthly average.
9. Calculate Pearson correlation and visualize the correlation heatmap and selected feature relationships.
10. Split data chronologically into 80% training and 20% testing, without shuffling.
11. Build Scikit-learn Pipelines with SimpleImputer(strategy="median") and each regression model. The imputer is fitted on training data only.
12. Train Linear Regression and Decision Tree Regression.
13. Evaluate using MAE, RMSE and R².
14. Plot predictions, residuals, metric comparisons and Decision Tree feature importance.
15. Save result tables, plots and trained pipelines.
Installation
Use Python 3. Install the required packages:
pip install pandas numpy matplotlib seaborn scikit-learn joblib
How to run
1. Save bank_customer_churn_system_exp10.py.
2. Ensure the computer has internet access because the script downloads the official UCI dataset.
3. Open a terminal in the script folder.
4. Run:
python bank_customer_churn_system_exp10.py
The script downloads the dataset directly from UCI, so a separate CSV download is not required.
Output folder
The script creates bank_customer_churn_system_exp10_outputs/ containing:
- Cleaned air-quality dataset.
- CO descriptive statistics and monthly statistics.
- Correlation matrix.
- Model performance comparison.
- Test-set predictions and residual summary.
- Decision Tree feature importance.
- EDA, trend, correlation, prediction, residual and model-comparison charts.
- Saved Linear Regression and Decision Tree pipelines (.joblib).
Results reported in the supplied PDF
Data preparation
- 114 completely blank rows removed.
- 16,701 sentinel missing values (-200) converted to NaN.
- 1,683 rows without valid CO target values removed.
- Final modeling dataset: 7,674 records and 10 columns (DateTime, eight predictors and target).
Chronological split
- Training: 6,139 records.
- Testing: 1,535 records.
- Training period: March 2004 to 25 January 2005.
- Testing period: 25 January 2005 to April 2005.
Model comparison
Model	Train MAE	Test MAE	Train RMSE	Test RMSE	Train R²	Test R²
Multivariate Linear Regression	0.3613	0.3834	0.5983	0.6416	0.8358	0.7723
Decision Tree Regression	0.0536	0.5849	0.3423	0.8790	0.9463	0.5725


According to the source PDF, Linear Regression generalizes better on the held-out chronological test data. The Decision Tree has much lower training error but worse test performance, indicating overfitting.
Decision Tree feature importance
The source PDF reports PT08.S2(NMHC) as the dominant feature, with an importance score of 0.8794 (87.94%).
Inference
The source experiment concludes that the multisensor features can estimate CO concentration and that the Linear Regression model is more robust on the chronological test window than the unconstrained Decision Tree. The PDF also notes that unconstrained Linear Regression can produce a small negative CO prediction, which is physically impossible; production systems may apply a non-negative output floor or a suitable target transformation.
Conclusion
The experiment demonstrates an end-to-end air-quality regression workflow using sensor telemetry. After cleaning sentinel values and invalid target records, exploratory analysis and correlation analysis are performed. Two regression models are trained using a chronological 80:20 split and training-only median imputation. Based on the reported test metrics, Multivariate Linear Regression performs better than the Decision Tree on unseen future observations. This practical demonstrates data preprocessing, EDA, regression modeling, evaluation and leakage-aware time-series validation.
References
1. De Vito, S., Massera, E., Piga, M., Martinotto, L., & Di Francia, G. (2008). On field calibration of an electronic nose for benzene estimation in an urban environment. Sensors and Actuators B: Chemical, 129(2), 750–757.
2. European Parliament and Council. (2008). Directive 2008/50/EC on ambient air quality and cleaner air for Europe.
3. Lewis, A., & Edwards, P. (2016). Validate personal air-pollution sensors. Nature, 535(7610), 29–31.
4. Spinelle, L., et al. (2015). Field calibration of a cluster of low-cost electrochemical and metal oxide gas sensors for air quality monitoring. IEEE Sensors Journal, 15(7), 3638–3647.
5. UCI Machine Learning Repository. (2016). Air Quality Data Set.
6. World Health Organization. (2021). WHO global air quality guidelines.
7. Scikit-learn documentation: Pipeline, SimpleImputer, LinearRegression and DecisionTreeRegressor.

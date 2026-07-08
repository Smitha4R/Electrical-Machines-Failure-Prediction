# Electrical-Machines-Failure-Prediction
In modern industrial environments, unscheduled equipment downtime is one of the most significant drivers of operational deficits, safety hazards, and inflated maintenance overhead. This project establishes an end-to-end Machine Learning Engineering (MLE) framework designed to shift factory maintenance strategies from reactive troubleshooting to proactive risk mitigation.

By leveraging multivariate time-series telemetry from embedded machinery sensors, this notebook implements a rigorous pipeline consisting of:

* Statistical & Collinearity Vetting: Validating data distributions via Shapiro-Wilk and Mann-Whitney U testing, alongside Spearman Rank correlation matrices to prevent feature redundancy.
* Imbalanced Class Classification: Combating extreme failure scarcity using heavily tuned ensemble models (Random Forest vs. Cost-Sensitive XGBoost).
* Explainable AI (XAI): Utilizing LIME (Local Interpretable Model-agnostic Explanations) to peel back the "black box" of complex algorithms, providing floor engineers with explicit, clear-text physical triggers for individual machine alarms.
* Deterministic Productionization: Serializing optimized pipeline artifacts for real-time inference wrappers.

## Dataset Overview
This dataset contains sensor data collected from various machines, with the aim of predicting machine failures in advance. It includes a variety of sensor readings as well as the recorded machine failures.

### Columns Description
- footfall: The number of people or objects passing by the machine.
- tempMode: The temperature mode or setting of the machine.
- AQ: Air quality index near the machine.
- USS: Ultrasonic sensor data, indicating proximity measurements.
- CS: Current sensor readings, indicating the electrical current usage of the machine.
- VOC: Volatile organic compounds level detected near the machine.
- RP: Rotational position or RPM (revolutions per minute) of the machine parts.
- IP: Input pressure to the machine.
- Temperature: The operating temperature of the machine.
- fail: Binary indicator of machine failure (1 for failure, 0 for no failure).

## Model Selection & Performance
Following extensive optimization, the Random Forest Classifier was selected as our champion architecture. Due to the high operational costs associated with missing a catastrophic failure (False Negatives), the pipeline was tuned to prioritize Recall without severely degrading Precision:

* Class 1 (Failure) Recall: 0.92 — Safely intercepting 92% of imminent machine breakdowns before physical manifestations occur.
* Class 1 (Failure) Precision: 0.88 — Maintaining a highly trustworthy alert system, keeping false alarms down to just 12%.
* ROC-AUC Score: 0.9810 — Demonstrating near-flawless separation capacity across all operating thresholds.
While the baseline XGBoost initially suffered a complete collapse due to the dataset's heavy class imbalance (yielding a 0.00 F1-score), tuning its internal loss-weighting parameters (scale_pos_weight = 1.40) successfully resuscitated its performance to a competitive 0.89 F1-score.

### 🔍 Root Cause Insights

Through tree-based global feature importances and targeted LIME localized diagnostics, a distinct physical failure signature was uncovered across the asset fleet:

- The Primary Trigger: Spikes in Volatile Organic Compounds (VOC) and ambient Air Quality (AQ) degradation act as the leading indicators of failure, mathematically contributing over 40% of the decision weight. This signals internal component off-gassing or lubricant thermal breakdown.
- The Structural Corroboration: Deeply negative Ultrasonic Proximity (USS) readings systematically accompany these chemical spikes, indicating internal mechanical shifting or warping.
- The Isolation Variable: Input Pressure (IP) remained consistently stable within normal bounds during failure events, systematically ruling out fluid line supply faults and confirming the breakdowns are entirely internal to the units.

### 🚀 Production Deployment Readiness

The entire pipeline has been fully decoupled from the training space. By serializing both the fitted StandardScaler and the champion Random Forest binaries via joblib, we successfully implemented a clean, warning-free production wrapper function (predict_machine_health). This function accepts raw telemetry streams, seamlessly constructs transient pandas payloads to satisfy feature-name alignment, and yields instant operational risk scores—providing an end-to-end blueprint ready for immediate integration into an industrial SCADA environment or web-based supervisory dashboard.

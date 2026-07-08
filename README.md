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

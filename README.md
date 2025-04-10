# AAE6102_Laboratory_Group7
# 1 Parameter Tuning and Effects
This section details the parameters tuned for four GNSS positioning algorithms in RTKlib (Single, DGPS, Kinematic, and PPP-Kinematic) and their effects on accuracy, processing speed, and robustness across Urban and Dynamic datasets.
## 1.1 Single Algorithm (Standard Point Positioning)
Parameters Tuned
Effects on Accuracy
AR Mode: Fix-and-Hold better in challenging environments; Continuous better in open-sky
AR Threshold: Higher values (2.5-3.5) improve reliability but reduce fix percentage
Process Noise: Lower values smoother; higher values track rapid movements better
Multiple GNSS: Improves accuracy by 25-45% in obstructed environments

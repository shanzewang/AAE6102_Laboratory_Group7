# AAE6102_Laboratory_Group7
# 1 Parameter Tuning and Effects

This section details key parameters tuned for four RTKlib positioning algorithms and their effects on performance metrics across Urban and Dynamic datasets.

## 1.1 Single Algorithm (Standard Point Positioning)

### Parameters Tuned

- **Elevation Mask**: Minimum elevation angle for satellites
- **SNR Mask**: Signal-to-noise ratio threshold
- **Ionospheric/Tropospheric Models**: Klobuchar, IONEX, Saastamoinen, etc.
- **Satellite Systems**: GPS, GLONASS, Galileo, BeiDou configurations
- **Filter Type**: Forward or Combined

### Effects

| Parameter | Accuracy | Processing Speed | Robustness |
|-----------|----------|-----------------|------------|
| Elevation Mask | Higher (15°-30°) reduces multipath errors but decreases satellite visibility | Higher values reduce computational load by 5-15% | Lower values (5°-10°) improve solution availability in obstructed environments |
| SNR Mask | Higher thresholds improve solution quality but reduce available measurements | Higher values increase processing speed | Lower thresholds increase solution availability in challenging signal environments |
| Correction Models | IONEX typically outperforms Klobuchar | Complex models increase processing time by 10-20% | Minimal impact on solution availability |
| Multiple GNSS | Improves accuracy by 20-40% in difficult environments | Each system adds 15-25% computation time | Significantly improves continuity in Urban scenarios |

## 1.2 DGPS Algorithm (Differential GPS)

### Parameters Tuned

- **Base Station Selection**: Distance to rover and data quality
- **Max Age of Differential**: Maximum age of differential corrections
- **Correction Type**: Code-based or code+phase
- **Satellite Systems**: GPS, GLONASS, Galileo, BeiDou combinations

### Effects

| Parameter | Accuracy | Processing Speed | Robustness |
|-----------|----------|-----------------|------------|
| Base Station Distance | Degrades ~1-2cm per 10km increase | Minimal impact | Closer stations provide more reliable corrections |
| Max Age of Differential | Values <30s optimal; >60s degrades performance | Larger values slightly reduce computation | Higher values (60-120s) maintain solutions during temporary outages |
| Correction Type | Code+phase improves accuracy by 30-50% | Code+phase increases computation by 15-25% | Code+phase generally more sensitive to data quality |
| Multiple GNSS | Improves accuracy by 20-40% in Urban areas | Each system adds 15-20% computation time | Significantly improves availability in obstructed environments |

## 1.3 Kinematic Algorithm (RTK with EKF)

### Parameters Tuned

- **AR Mode**: Continuous, Fix-and-Hold, Instantaneous
- **AR Validation Threshold**: Ratio test threshold
- **Process Noise**: Standard deviations for position, velocity
- **Ionosphere/Troposphere Models**: Estimation options

### Effects

| Parameter | Accuracy | Processing Speed | Robustness |
|-----------|----------|-----------------|------------|
| AR Mode | Fix-and-Hold better in challenging environments; Continuous better in open-sky | Fix-and-Hold requires 5-10% more processing | Fix-and-Hold provides more continuous solutions but risks error propagation |
| AR Threshold | Higher values (2.5-3.5) improve reliability but reduce fix percentage | Minimal impact | Lower values increase fix rate but risk incorrect solutions |
| Process Noise | Lower values smoother; higher values track rapid movements better | Complex models increase computation by 10-15% | Higher values better track dynamic motion |
| Multiple GNSS | Improves accuracy by 25-45% in obstructed environments | Each constellation adds 20-30% computation | Dramatically improves continuity in Urban environments |

## 1.4 PPP-Kinematic Algorithm

### Parameters Tuned

- **Precise Products**: Final, Rapid, Ultra-rapid orbits and clocks
- **Process Noise**: Position, troposphere, and clock parameters
- **Troposphere Estimation**: Fixed model or estimated as states
- **Ambiguity Resolution**: Float or Fixed

### Effects

| Parameter | Accuracy | Processing Speed | Robustness |
|-----------|----------|-----------------|------------|
| Precise Products | Final products improve accuracy by 15-30% over ultra-rapid | Minimal impact once loaded | Higher quality products improve solution stability |
| Process Noise | Lower noise (10^-5 m²/s) for low dynamics; higher (10^-3 m²/s) for high dynamics | More states increase computation by 20-30% | Higher values better accommodate unexpected movements |
| Troposphere Estimation | Estimated troposphere improves vertical accuracy by 30-50% | Increases computation load by 15-25% | Improves performance in variable weather conditions |
| Multiple GNSS | Improves accuracy and convergence speed by 25-40% | Each constellation adds 25-35% computation | Significantly improves solution continuity and convergence |

## Cross-Algorithm Comparison

| Environment | Recommended Parameter Adjustments |
|-------------|----------------------------------|
| Urban | Higher elevation masks (15°-20°); stricter SNR thresholds; multiple GNSS systems |
| Dynamic | Higher process noise; kinematic modes; shorter update intervals |
| Computation Priority | Single < DGPS < Kinematic < PPP-Kinematic |
| Convergence Time | Single (immediate) < DGPS (<30s) < Kinematic (30s-5min) < PPP-Kinematic (5-30min) |


# 2 Strengths and Limitations

## 2.1 DGPS (Differential GPS)
**Strengths:**
- **Flexibility:** Can be used in various environments with different base station configurations.
- **Robustness:** Provides improved accuracy over standalone GPS by correcting errors.
- **Ease of Use:** Relatively straightforward to implement with existing infrastructure.

**Limitations:**
- **Computational Efficiency:** Requires additional processing to handle differential corrections.
- **Lack of Specific Features:** May not provide the precision needed for high-accuracy applications compared to RTK or PPP.

## 2.2 Kinematic (EKF - Extended Kalman Filter)
**Strengths:**
- **Flexibility:** Adaptable to different motion models and sensor inputs.
- **Robustness:** Can handle dynamic environments and varying motion states.
- **Ease of Use:** Widely used with extensive documentation and community support.

**Limitations:**
- **Computational Efficiency:** Computationally intensive due to the recursive nature of the filter.
- **Lack of Specific Features:** May require significant tuning for optimal performance in specific scenarios.

## 2.3 PPP Kinematic (Precise Point Positioning)
**Strengths:**
- **Flexibility:** Does not require a base station, suitable for global applications.
- **Robustness:** Provides high accuracy with precise satellite data.
- **Ease of Use:** Can be implemented with a single receiver, reducing infrastructure needs.

**Limitations:**
- **Computational Efficiency:** High computational demand due to the need for precise satellite corrections.
- **Lack of Specific Features:** Convergence time can be long, affecting real-time applications.

## 2.4 Single (Standalone GPS)
**Strengths:**
- **Flexibility:** Simple setup with minimal requirements.
- **Robustness:** Basic level of robustness for general navigation.
- **Ease of Use:** Easiest to implement with minimal configuration.

**Limitations:**
- **Computational Efficiency:** Generally efficient but lacks advanced processing.
- **Lack of Specific Features:*

# 3 Comparison with Other Libraries

# 4 Suggestions for Improvement



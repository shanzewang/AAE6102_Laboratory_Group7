# AAE6102_Laboratory_Group7
# 1 Parameter Tuning and Effects

This section details key parameters tuned for four RTKlib positioning algorithms and their effects on performance metrics across Urban dataset.

## 1.1 DGPS Algorithm (Differential GPS)

## Parameter Adjustments and Effects
The adjusted parameters are shown in the figure below:

![image](https://github.com/shanzewang/AAE6102_Laboratory_Group7/blob/main/DGPSDGNSS/setting1.png) ![image](https://github.com/shanzewang/AAE6102_Laboratory_Group7/blob/main/DGPSDGNSS/setting2.png)



### Reduction of Max Age of Differential Corrections (from 30.0s to 10.0s)

#### Positive Effects:

- **Improved Positioning Accuracy**: Newer differential data reduces clock and orbital errors, potentially decreasing positioning bias from decimeter to centimeter level.

- **Enhanced Ambiguity Resolution Rate**: More current differential data aids ambiguity resolution, potentially transforming float solutions (Q=5) to fixed solutions (Q=1).

- **Reduced Systematic Errors**: Older differential data can introduce systematic biases; shorter delay times mitigate this issue.

#### Potential Risks:

- **Increased Solution Failure Rate**: If network delays frequently exceed 10 seconds, excessive differential data might be discarded, causing solution interruptions or convergence issues.

- **Higher Communication Requirements**: A more stable data transmission link (e.g., faster network or direct radio transmission) becomes necessary.

### Adjustment of Outlier Threshold for Code/Phase (from 30.0/5.0m to 20.0/3.0m)

#### Positive Effects:

- **Enhanced Positioning Precision**: Stricter outlier detection eliminates anomalous observations caused by multipath effects and signal interference, reducing pseudorange and carrier phase noise. Pseudorange errors may decrease from meter to decimeter level, while carrier phase errors may reduce to centimeter level.

- **Improved Ambiguity Resolution**: Reduced carrier phase noise facilitates ambiguity resolution, potentially converting float solutions (Q=5) to fixed solutions (Q=1).

- **Greater Solution Stability**: Elimination of anomalous observations leads to more stable solutions and smoother trajectories.

#### Potential Risks:

- **Reduced Available Observations**: Lower thresholds might incorrectly classify valid observations as outliers, decreasing the number of usable satellites. However, with 14-16 satellites in the dataset, this impact may be minimal.

- **Increased Solution Failure Risk**: In high-noise environments (e.g., urban canyons), excessive elimination of observations might lead to solution failures, which can be verified by examining changes in satellite count (ns) and quality factor (Q).

The comparison results of the parameters are shown in the following figure:

![image](https://github.com/shanzewang/AAE6102_Laboratory_Group7/blob/main/DGPSDGNSS/figs/compare.png)



## 1.2 Kinematic Algorithm (RTK with EKF)

## Parameter Adjustments and Effects
The adjusted parameters are shown in the figure below:

![image](https://github.com/shanzewang/AAE6102_Laboratory_Group7/blob/main/Kinematic/setting1.png) ![image](https://github.com/shanzewang/AAE6102_Laboratory_Group7/blob/main/Kinematic/setting2.png)

### Cycle Slip Detection Threshold (Slip Thresh: Doppler/Geom-Free)

#### Configuration Changes:

- Default: 0.000 Hz / 0.050 m
- Alternative: 0.005 Hz / 0.100 m

#### Expected Effects:

- A Doppler threshold of 0.005 Hz (vs. 0.000 Hz) was expected to reduce false cycle slip detections, particularly in dynamic environments with vibration and acceleration changes.
- Increasing the Geometry-Free carrier phase threshold from 0.050 m to 0.100 m was expected to maintain carrier phase observation continuity while potentially allowing some additional noise.

#### Actual Results:

- Contrary to expectations, the stricter threshold setting (0.000 Hz / 0.050 m) produced better results.
- This suggests that in this urban dataset, the stricter cycle slip detection improved solution quality by effectively eliminating problematic measurements.

### Maximum Age of Differential Corrections

#### Configuration Changes:

- Default: 10.0 seconds
- Alternative: 5.0 seconds

#### Expected Effects:

- Reducing the maximum age from 10.0 to 5.0 seconds was expected to improve data timeliness in Kinematic mode, potentially enhancing precision and ambiguity resolution.
- This adjustment might have increased solution failure rates depending on communication delays.

#### Actual Results:

- No significant difference was observed between the 10.0 and 5.0 second settings.
- This suggests that the communication link was stable with delays consistently below 5.0 seconds, making this parameter adjustment less impactful for this dataset.

### Minimum Satellites for Fixing/Holding Ambiguities

#### Configuration Changes:

- Default: 4 / 5 satellites
- Alternative: 5 / 5 satellites

#### Expected Effects:

- Increasing the minimum satellite requirement for ambiguity resolution from 4 to 5 was expected to improve solution stability in dynamic environments.
- This adjustment might have reduced solution availability in areas with limited satellite visibility.

#### Actual Results:

- No significant performance difference was observed between the two configurations.
- This suggests that satellite availability was sufficient throughout the test area, with the number of visible satellites consistently above the threshold values in both configurations.

The comparison results of the parameters are shown in the following figure:

![image](https://github.com/shanzewang/AAE6102_Laboratory_Group7/blob/main/Kinematic/figs/compare.png)

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
- **Flexibility:** DGPS can be adapted to various operational scenarios, including both stationary and mobile applications. It can be integrated with existing GPS infrastructure, making it versatile for different use cases.
- **Robustness:** By using correction data from a nearby base station, DGPS significantly reduces errors caused by atmospheric conditions and satellite clock drift, enhancing positional accuracy.
- **Ease of Use:** The setup for DGPS is relatively straightforward, especially when using commercial services that provide correction data, reducing the complexity for end-users.

**Limitations:**
- **Computational Efficiency:** The need to process correction data in real-time can increase computational demands, particularly in environments with limited processing power.
- **Lack of Specific Features:** While DGPS improves accuracy, it may not achieve the centimeter-level precision required for applications like precision agriculture or autonomous vehicle navigation.


## 2.2 Kinematic (EKF - Extended Kalman Filter)
**Strengths:**
- **Flexibility:** The EKF can be tailored to various dynamic models, making it suitable for applications ranging from pedestrian navigation to high-speed vehicular tracking.
- **Robustness:** EKF is capable of filtering out noise and providing stable estimates even in environments with erratic motion or signal loss.
- **Ease of Use:** With extensive literature and community support, implementing EKF is accessible for developers, and many software libraries offer built-in support.

**Limitations:**
- **Computational Efficiency:** The recursive nature of the EKF, involving matrix operations, can be computationally expensive, especially for systems with limited resources.
- **Lack of Specific Features:** EKF requires careful tuning of noise parameters, and its performance can degrade if the model assumptions do not match the real-world scenario.


## 2.3 Single Algorithm (Standard Point Positioning)
**Strengths:**
- **Flexibility:** Standard Point Positioning is the most basic form of positioning, requiring only a GPS receiver, making it highly accessible and easy to deploy.
- **Robustness:** Provides a basic level of robustness for general navigation tasks where high precision is not critical.
- **Ease of Use:** With minimal setup and no need for additional data sources, standard point positioning is user-friendly and cost-effective.

**Limitations:**
- **Computational Efficiency:** While efficient in terms of processing, standard point positioning lacks the advanced error correction capabilities of other methods.
- **Lack of Specific Features:** It is susceptible to errors from atmospheric conditions, multipath effects, and satellite geometry, limiting its accuracy.


# 3 Comparison with Other Libraries

For this comparison, we will hypothetically compare RTKlib with NovAtel's Waypoint Software Suite, a well-known commercial GNSS library.

## 3.1 Comparison Criteria

### 3.1.1. Accuracy
- **RTKlib**:  
  RTKlib can achieve high accuracy in open-sky environments, especially when parameters are carefully tuned. However, in urban or dynamic environments with significant multipath effects or signal obstructions, its performance can degrade. Achieving consistent accuracy often requires extensive parameter adjustments by the user.  

- **Waypoint Software**:  
  This commercial solution provides consistently high accuracy in a variety of environments, including urban canyons and high-dynamic scenarios. Its proprietary algorithms and advanced error modeling techniques (e.g., multipath mitigation and robust ambiguity resolution) ensure superior performance with minimal user intervention.  

**Conclusion**: While RTKlib performs well in ideal conditions, Waypoint Software is more accurate and reliable in challenging environments.

---

### 3.1.2. Ease of Use
- **RTKlib**:  
  RTKlib has a steep learning curve. Users must manually configure input files, adjust parameters, and interpret results. It lacks a modern graphical interface (GUI), which makes it less accessible to beginners or non-specialists.  

- **Waypoint Software**:  
  Waypoint Software provides an intuitive GUI with built-in tools for parameter configuration, visualization, and automated optimization. This makes it user-friendly and suitable for a broader audience, including those without advanced GNSS expertise.  

**Conclusion**: Waypoint Software is significantly easier to use than RTKlib, especially for users with limited technical knowledge.

---

### 3.1.3. Flexibility
- **RTKlib**:  
  As an open-source library, RTKlib offers unparalleled flexibility. Users can modify the source code, add support for new algorithms or GNSS constellations, and customize the software for specific use cases. This makes it ideal for research and development.  

- **Waypoint Software**:  
  As a proprietary solution, Waypoint Software is less flexible. Users are limited to the features and algorithms provided by the vendor, and customization is generally not possible.  

**Conclusion**: RTKlib is far more flexible than Waypoint Software due to its open-source nature and modifiability.

---

### 3.1.4. Computational Efficiency
- **RTKlib**:  
  RTKlib's computational efficiency is moderate. While it is suitable for post-processing on standard hardware, it can be slower in real-time applications or when using advanced error correction models (e.g., IONEX, Saastamoinen).  

- **Waypoint Software**:  
  Waypoint Software is highly optimized for both post-processing and real-time applications. Its proprietary algorithms and efficient implementation allow it to handle large datasets or high-dynamic scenarios with minimal latency.  

**Conclusion**: Waypoint Software is more computationally efficient than RTKlib, particularly in real-time applications or scenarios involving large datasets.

---

## 3.2 Summary of Comparison

| **Criteria**          | **RTKlib**                      | **Waypoint Software**              |
|------------------------|----------------------------------|-------------------------------------|
| **Accuracy**           | High in open-sky environments but degrades in challenging conditions | Consistently high in all environments |
| **Ease of Use**        | Steep learning curve, lacks modern GUI | User-friendly with intuitive GUI and automation |
| **Flexibility**        | Open-source, highly customizable | Closed-source, limited customization |
| **Computational Efficiency** | Moderate, slower for real-time or large datasets | Highly optimized for real-time and post-processing |

**Overall Assessment**:  
RTKlib is an excellent choice for researchers and developers who prioritize flexibility and cost-effectiveness. However, for users seeking ease of use, high accuracy in challenging conditions, and optimized performance, commercial solutions like Waypoint Software are more suitable.

---







# 4 Suggestions for Improvement


To make RTKlib more competitive and user-friendly, the following improvements are recommended:

## 4.1 Enhancing Accuracy
1. **Advanced Multipath Mitigation**:  
   Introduce more sophisticated multipath error modeling and mitigation techniques to improve accuracy in urban environments. This could include leveraging machine learning methods to identify and filter multipath signals.
   
2. **Improved Ambiguity Resolution**:  
   Develop more robust ambiguity resolution algorithms to ensure higher accuracy and reliability in dynamic or obstructed environments.

---

## 4.2 Improving Ease of Use
1. **Modern GUI Development**:  
   Add a modern, user-friendly graphical interface for parameter configuration, data input, and result visualization. This would lower the entry barrier for new users.  

2. **Preconfigured Templates**:  
   Provide preconfigured parameter templates for common use cases (e.g., urban navigation, dynamic tracking), reducing the need for manual tuning.

3. **Comprehensive Documentation and Tutorials**:  
   Expand the documentation to include step-by-step guides, real-world examples, and troubleshooting tips. Video tutorials and interactive manuals could also be helpful.

---

## 4.3 Increasing Flexibility
1. **Plugin System**:  
   Implement a plugin system that allows users to extend RTKlib’s functionality without modifying the core codebase. This would enable the community to share and reuse custom algorithms or features.  

2. **Support for Emerging GNSS Constellations**:  
   Expand support for new GNSS constellations like QZSS and IRNSS to increase flexibility and improve positioning accuracy worldwide.

---

## 4.4 Enhancing Computational Efficiency
1. **Parallel Processing**:  
   Optimize RTKlib to use parallel processing techniques, such as multi-threading, to reduce processing times for large datasets or real-time applications.  

2. **Cloud-Based Processing**:  
   Provide an option for cloud-based processing, allowing users to offload computationally intensive tasks to the cloud and access results on demand.  

---

## 4.5 Other Suggestions
1. **Machine Learning Integration**:  
   Use machine learning models for post-processing tasks like ambiguity resolution or error correction to improve performance in complex scenarios.  

2. **Mobile and Lightweight Versions**:  
   Develop a lightweight version of RTKlib for mobile platforms, enabling real-time GNSS processing on smartphones or tablets.  

3. **Regular Updates and Maintenance**:  
   Ensure that RTKlib is actively maintained with frequent updates, bug fixes, and new features to keep it competitive with commercial solutions.

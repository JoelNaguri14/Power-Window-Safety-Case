# 🚗 SOTIF Triggering Condition Analysis — Automatic Emergency Braking (AEB)

> **An independent ISO 21448 (SOTIF) analysis identifying five triggering conditions for an Automatic Emergency Braking system across real-world UK operational scenarios.**

---

## 📋 Project Overview

This repository documents a **SOTIF (Safety Of The Intended Functionality)** analysis conducted on an Automatic Emergency Braking (AEB) system in accordance with **ISO 21448**.

Unlike ISO 26262 — which addresses random hardware failures — SOTIF specifically addresses scenarios where **hardware functions correctly** but the system still causes harm due to **performance limitations, sensor constraints, or unexpected environmental conditions** outside the system's operational design domain.

Five triggering conditions are identified across distinct UK operational scenarios, each with a specific failure mechanism, hazard description, and sensor fusion mitigation strategy.

---

## 🔑 SOTIF vs ISO 26262 — Key Distinction

| Standard | Addresses |
|---|---|
| **ISO 26262** | Random hardware failures — component malfunctions |
| **ISO 21448 (SOTIF)** | Performance limitations and misuse — hardware works correctly but system still causes harm due to sensor limitations or unexpected environments |

---

## ⚠️ Triggering Condition Analysis

### TC-01 — Low Ambient Light and Heavy Rainfall

**Scenario:** Vehicle operating on an unlit urban road in heavy rainfall with standing water on road surface.

**Triggering Condition:**
Low ambient light reduces camera pixel contrast below the object detection threshold of the perception algorithm, causing false-negative obstacle classification.

**Failure Mechanism:** Camera-based perception — optical limitation under low photon availability.

**Hazard:** AEB system fails to activate — vehicle collides with obstacle at speed.

**SOTIF Scenario Classification:** Known unsafe scenario — performance limitation under defined environmental condition.

**Mitigation:**
> Sensor fusion — cross-reference camera data with radar, which is significantly less affected by rain and low light. When camera confidence drops below detection threshold, radar assumes primary detection role automatically.

---

### TC-02 — Motorway Congestion (M25 Rush Hour)

**Scenario:** High-density urban motorway during peak congestion — M25, London.

**Triggering Condition:**
Multiple overlapping bounding boxes in the object detection model cause classification confidence to drop below the AEB activation threshold. System is unable to distinguish stationary traffic from moving vehicles at close range.

**Failure Mechanism:** Algorithm limitation — object classification model degradation under high object density.

**Hazard:** AEB activates too late or fails to activate — rear-end collision at motorway speed.

**SOTIF Scenario Classification:** Known unsafe scenario — algorithmic performance limitation under dense traffic ODD.

**Mitigation:**
> Adaptive processing algorithms increase sensor sampling frequency in high-density traffic scenarios. Redundant radar provides independent distance measurement decoupled from visual classification confidence.

---

### TC-03 — Snow and Freezing Fog (Scottish A9 Corridor)

**Scenario:** Highland driving in winter conditions — Scotland A9 corridor.

**Triggering Condition:**
Heavy snowfall and freezing fog simultaneously accumulate on the radar radome, reducing signal return strength by up to 40%, causing false-negative obstacle detection.

**Failure Mechanism:** Radar physical limitation — radome contamination attenuating signal return.

**Hazard:** AEB fails to detect obstacle — collision at rural road speed.

**SOTIF Scenario Classification:** Known unsafe scenario — sensor performance degradation under combined environmental condition.

**Mitigation:**
> LiDAR introduced as a third independent sensing channel, providing point cloud detection independent of optical or RF conditions. System issues driver alert to assume manual control when all sensor confidence scores fall below the defined safe threshold simultaneously.

---

### TC-04 — Low Sun Glare (Evening Commute)

**Scenario:** Vehicle travelling westward during sunset on a dual carriageway — common UK evening commute scenario.

**Triggering Condition:**
Direct solar radiation at low angle (0–15°) saturates the camera image sensor, causing complete loss of forward object detection capability. Sun glare in the 0–15° angle range creates maximum sensor saturation conditions.

**Failure Mechanism:** Camera sensor saturation — optical overload from direct solar radiation at critical approach angle.

**Hazard:** AEB fails to detect slow-moving or stationary vehicle ahead — high-speed rear-end collision.

**SOTIF Scenario Classification:** Known unsafe scenario — predictable geometric condition creating sensor performance boundary.

**Mitigation:**
> Camera exposure algorithm automatically transitions to High Dynamic Range (HDR) mode upon detection of glare conditions. Radar assumes primary detection role when camera object detection confidence falls below 60% threshold.

---

### TC-05 — Unusual Road Obstacles — Large Animals (Rural UK)

**Scenario:** Rural road encounter with unexpected large animal — common in Scottish Highlands and Welsh countryside.

**Triggering Condition:**
AEB object classification algorithm, trained predominantly on vehicle and pedestrian datasets, fails to correctly classify large animals (deer, cattle, horses) as obstacles requiring emergency braking. Classification confidence falls below AEB activation threshold.

**Failure Mechanism:** Machine learning model limitation — out-of-distribution object classification failure.

**Hazard:** AEB fails to activate — vehicle collides with large animal at rural road speed, causing significant vehicle damage and potential occupant injury.

**SOTIF Scenario Classification:** Known unsafe scenario — algorithmic performance boundary due to training data distribution mismatch.

**Mitigation:**
> Object classification model retrained with diverse obstacle datasets including animals and non-standard road objects. Any unclassified object above a defined minimum size threshold is treated as a potential hazard, triggering precautionary deceleration regardless of classification confidence score.

---

## 📊 Summary Table

| TC | Scenario | Root Cause Type | Primary Sensor Affected | Mitigation Strategy |
|---|---|---|---|---|
| TC-01 | Low light + rain | Optical limitation | Camera | Radar primary takeover |
| TC-02 | M25 congestion | Algorithm degradation | Camera (classification) | Adaptive sampling + radar |
| TC-03 | Snow + fog (A9) | Physical contamination | Radar | LiDAR third channel + driver alert |
| TC-04 | Low sun glare | Sensor saturation | Camera | HDR mode + radar takeover |
| TC-05 | Large animals (rural) | ML model OOD failure | Camera (classification) | Dataset retraining + size-threshold rule |

---

## 🔍 Analysis Conclusions

This analysis identifies five SOTIF triggering conditions for an AEB system across realistic UK operational scenarios. Each condition represents a **performance limitation** where hardware functions correctly but system safety cannot be guaranteed due to environmental or scenario factors outside the system's operational design envelope.

Key findings:
- **Camera** is the most vulnerability-prone sensor across all five TCs — optical limitations dominate
- **Sensor fusion** (camera + radar + LiDAR) is the primary mitigation architecture throughout
- **Machine learning classification models** introduce a distinct category of SOTIF risk beyond traditional sensor physics
- Graceful degradation with driver notification is the appropriate safe state when all sensor confidence falls below threshold simultaneously

---

## 🛠️ Skills Demonstrated

`ISO 21448` `SOTIF` `Triggering Condition Analysis` `Operational Design Domain` `Sensor Fusion` `Perception System Safety` `AEB Systems` `Camera/Radar/LiDAR` `ML Model Safety` `ISO 26262`

---

## 👤 Author

**Sunder Joel Naguri**
MSc Mechatronics and Computer Systems Engineering — University of East London
📧 nagurijoel@gmail.com
🔗 [LinkedIn](https://linkedin.com/in/your-profile) | [GitHub Portfolio](https://github.com/JoelNaguri14)

*Currently on UK Graduate Route Visa (valid until October 2027) — available to start immediately — eligible for Skilled Worker sponsorship*


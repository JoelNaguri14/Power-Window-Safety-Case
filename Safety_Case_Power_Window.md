# 🔒 Power Window Functional Safety Case — ISO 26262

> **A complete functional safety lifecycle analysis of an automotive power window system, conducted in accordance with ISO 26262 Parts 3 and 4.**

---

## 📋 Project Overview

This repository documents a full concept-phase safety analysis for an automotive **Power Window System**, demonstrating end-to-end application of the ISO 26262 functional safety standard — from Item Definition through to Architecture Design with justified redundancy.

The analysis identifies **Unintended Window Closing** as an **ASIL D** hazard and derives a complete safety case including Safety Goals, Functional Safety Requirements, Technical Safety Requirements, and a justified 1oo2 diverse redundancy architecture.

---

## 🗂️ Repository Contents

| Document | Description |
|---|---|
| `HARA_Table.xlsx` | Full Hazard Analysis and Risk Assessment with ASIL determination |
| `Safety_Goals_FSR.pdf` | Safety Goal SG-01 and Functional Safety Requirements |
| `TSR_Traceability_Matrix.xlsx` | Technical Safety Requirements with full traceability to FSRs |
| `Architecture_1oo2.png` | Diverse redundancy architecture diagram |
| `README.md` | This document |

---

## 🔧 System Description — Item Definition

**Item:** Automotive Power Window System

**System Boundary — Key Interfaces:**
- **Door Control Unit (DCU)** — primary control logic
- **Hall Effect Sensors** — window position and speed sensing (Channel B)
- **Motor Current Monitor** — obstruction detection via current spike (Channel A)
- **H-Bridge Driver** — motor direction and power control
- **Window Motor** — mechanical actuation

---

## ⚠️ Hazard Analysis and Risk Assessment (HARA)

**Identified Hazard:** Unintended window closing caused by malfunctioning behaviour of the Door Control Unit.

**Operational Situation:** Child passenger in rear seat during a normal drive cycle, with driver unable to monitor the rear window.

| Parameter | Rating | Justification |
|---|---|---|
| **Severity (S)** | S3 | Unintended closing can cause neck entrapment resulting in asphyxiation or cervical spine injury — AIS 4–6, potentially fatal |
| **Exposure (E)** | E4 | Operational situation occurs in almost every drive cycle — passengers routinely interact with the power window system |
| **Controllability (C)** | C3 | Driver cannot detect a rear-seat obstruction and cannot override the window motor before injury occurs |
| **ASIL Determination** | **ASIL D** | S3 + E4 + C3 = ASIL D per ISO 26262-3 Table 4 |

---

## 🎯 Safety Goal

**SG-01:** The power window system shall prevent clamping forces on any occupant from exceeding **100N** during window closing — **ASIL D**

**Fault Tolerant Time Interval (FTTI): 200ms**
- Sensor acquisition: ~10ms
- Processing and fault detection: ~50ms
- Actuator response (motor reversal): ~140ms
- Total: ≤ 200ms

---

## 📐 Functional Safety Requirements (FSRs)

| ID | Requirement |
|---|---|
| **FSR-01** | The power window system shall detect an obstruction in the window path within the Fault Tolerant Time Interval (FTTI) of 200ms |
| **FSR-02** | Upon detection of an obstruction, the power window system shall transition to the safe state by reversing motor direction within the FTTI of 200ms |
| **FSR-03** | The system shall enter a defined safe state (motor power removed) upon detection of any safety-relevant fault |
| **FSR-04** | The safe state shall be maintained until a deliberate driver reset action is performed |

---

## ⚙️ Technical Safety Requirements (TSRs)

| ID | Requirement | Traces To |
|---|---|---|
| **TSR-01** | The microcontroller shall trigger a fault if motor current exceeds **15A** for more than **10ms** debounce time | FSR-01 |
| **TSR-02** | The H-bridge driver shall sever power to the motor and reverse motor direction independently of the main control logic upon fault detection | FSR-02 |
| **TSR-03** | The Hall Effect sensor shall independently monitor window closing speed — fault declared if speed continues without corresponding current drop | FSR-01 |
| **TSR-04** | Both Channel A and Channel B shall complete their fault detection cycle within 100ms of obstruction contact | FSR-01, FSR-02 |

---

## 🏗️ Architecture Decision — 1oo2 Diverse Redundancy

### Why Redundancy is Required

Per ISO 26262, achieving **ASIL D** with a single microcontroller or single sensor is mathematically insufficient. A single-channel system's baseline FIT (Failures In Time) rate cannot meet the ASIL D hardware architectural metric requirements. Redundancy is mandatory.

### Architecture Design

Two independent sensing channels monitor the system in parallel. Either channel detecting a fault independently initiates the safe state:

```
┌─────────────────────────────────────────────────┐
│              POWER WINDOW SYSTEM                │
│                                                 │
│  ┌──────────────┐      ┌──────────────────────┐ │
│  │  CHANNEL A   │      │      CHANNEL B        │ │
│  │ Current      │      │  Hall Effect Speed   │ │
│  │ Monitoring   │      │  Sensing             │ │
│  │              │      │                      │ │
│  │ Fault if     │      │ Fault if speed       │ │
│  │ I > 15A      │      │ continues w/o        │ │
│  │ for > 10ms   │      │ current response     │ │
│  └──────┬───────┘      └──────────┬───────────┘ │
│         │                         │              │
│         └──────────┬──────────────┘              │
│                    │  Either channel faults       │
│                    ▼                             │
│          ┌─────────────────┐                    │
│          │   SAFE STATE    │                    │
│          │ Motor reversed  │                    │
│          │ within 200ms    │                    │
│          └─────────────────┘                    │
└─────────────────────────────────────────────────┘
```

### Why 1oo2 and Not 2oo3

| Architecture | When to Use |
|---|---|
| **2oo3** | When both safety AND availability are critical — system must never false-trip AND never miss a real fault (e.g. nuclear, aircraft flight control) |
| **1oo2** | When safety is paramount and false-trip consequence is acceptable |

**Justification:** For a power window system, the consequence of a spurious safe state activation is minimal — the window simply reverses. There is no availability penalty significant enough to justify the added cost and complexity of 2oo3. **1oo2 provides sufficient diagnostic coverage at proportionate cost and complexity** — a deliberate and justified engineering trade-off.

---

## ✅ Requirements Traceability Summary

```
SG-01 (ASIL D)
  ├── FSR-01 (Detect obstruction within FTTI)
  │     ├── TSR-01 (Current > 15A / 10ms → fault)
  │     └── TSR-03 (Hall Effect speed monitoring)
  ├── FSR-02 (Reverse motor within 200ms)
  │     └── TSR-02 (H-bridge independent reversal)
  ├── FSR-03 (Enter safe state on fault)
  │     └── TSR-04 (Both channels complete within 100ms)
  └── FSR-04 (Maintain safe state until reset)
```

---

## 🛠️ Skills Demonstrated

`ISO 26262` `HARA` `ASIL Determination` `Safety Goals` `FSR Derivation` `TSR Derivation` `1oo2 Architecture` `FIT Rate Analysis` `FTTI Calculation` `Requirements Traceability` `V-Model Lifecycle` `Diverse Redundancy`

---

## 👤 Author

**Sunder Joel Naguri**
MSc Mechatronics and Computer Systems Engineering — University of East London
📧 nagurijoel@gmail.com
 [GitHub Portfolio](https://github.com/JoelNaguri14)

*Currently on UK Graduate Route Visa (valid until October 2027) — available to start immediately — eligible for Skilled Worker sponsorship*


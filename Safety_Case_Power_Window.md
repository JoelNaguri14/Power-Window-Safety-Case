HARA - Power Window system
ITEM DEFINITION- Automotive power window system
HAZARD- Unintended window closing caused by malfunctioning behaviour of the door control  unit.
OPERATIONAL SITUATION- child passenger in rear seat during normal drive cycle with driver unable to monitor rear window.
Severity-S3-Unintended closing can cause neck entrapment resulting in asphyxiation or cervical injury -AIS-4-6, potentially fatal.
Exposure- E4- the operation situation occurs in almost every drive cycle as passengers routinely interact with the power windows system
Controllability-C3- The driver cannot detect a rear seat obstruction and override the window motor before injury occurs.
ASIL DETERMINATION:- S3+E4+C3 = ASIL D
SAFETY GOAL- The power window system shall prevent clamping forces on any occupant from exceeding 100N during window closing. ASIL D.

FSR :-
FSR-01:The power window system shall detect an obstruction in the window path within the fault tolerant time interval.
FSR-02: Upon detection of an obstruction, the power window system shall transition to the safe state by reversing the motor direction within the FTTI of 200ms.

TSR :-
1.	TSR-01: The microcontroller shall trigger a fault if motor current exceeds 15A for more than 10ms debounce time.
2.	TSR-02: The H-bridge driver shall be capable of severing power to the motor and reversing motor direction independently of the main control logic upon fault detection.

Architecture decision: 1oo2 Diverse Redundancy
According to ISO 26262, to achieve ASILD rating a single microcontroller or a single sensor is mathematically insufficient due to its baseline failure rate(FIT rate) we must implement redundancy. two independent sensors monitor the system then process data in parallel. if either Channel A-Current Monitoring (or) Channel B-Hall effect speed Sensing detects a fault, the system shall initiate a safe state.  we have choose 1002 instead of 2oo3 because 2oo3 is used when you need both high safety and high reliability. i.e system must never give a false trip and never miss a real fault.For a Power Window system, the consequence of a spurious safe state activation is low — the window simply reverses. Therefore 1oo2 provides sufficient diagnostic coverage at proportionate cost and complexity

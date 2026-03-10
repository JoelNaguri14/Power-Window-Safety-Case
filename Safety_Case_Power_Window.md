HARA -POWER WINDOW SYSTEM
ITEM DEFINITION-AUTOMOTIVE POWER WINDOW SYSTEM
HAZARD- Unintended window closing caused by malfunctioning behaviour of the door control  unit.
OPERATIONAL SITUATION- child passenger in rear seat during normal drive cycle with driver unable to monitor rear window.
Severity-S3-Unintended closing can cause neck entrapment resulting in asphyxiation or cervical injury -AIS-4-6, potentially fatal.
Exposure- E4- the operation situation occurs in almost every drive cycle as passengers routinely interact with the power windows system
Controllability-C3- The driver cannot detect a rear seat obstruction and override the window motor before injury occurs.
ASIL DETERMINATION:- S3+E4+C3= ASIL D
SAFETY GOAL- The power window system shall prevent clamping forces on any occupant from exceeding 100N during window closing. ASIL D.
FSR :-
1.	The power window system shall detect an obstruction in the window path within the fault tolerant time interval.
2.	UPON DETECTION OF AN OBSTRUCTION, THE POWER WINDOW SYSTEM SHALL TRANSITION TO THE SAFE STATE BY REVERSING THE MOTOR DIRECTION WITHIN THE FTTI OF 200MS.


TSR:-
1.	TSR-01: The microcontroller shall trigger a fault if motor current exceeds 15A for more than 10ms debounce time.
2.	TSR-02: The H-bridge driver shall be capable of severing power to the motor and reversing motor direction independently of the main control logic upon fault detection.


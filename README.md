# IVS Black-Box Test Automation

## Overview
This repository is a **Vector CANoe/CAPL-based black-box test automation project** for validating IVS diagnostic behavior.
The project focuses on verifying whether diagnostic requirements are satisfied under boundary conditions by injecting CAN signals, requesting fault status, and checking detection, healing, and erase behavior without modifying internal ECU logic.

In particular, this repository currently centers on battery-related diagnostic scenarios and organizes the test flow so that repetitive validation can be executed in a consistent and traceable way.

---

## Project Goal
The goal of this project is to automate validation of diagnostic requirements such as:

- fault **non-detection** under normal conditions
- fault **detection** under boundary and limit conditions
- fault **healing** after recovery conditions are met
- DTC **erase** after the specified ignition cycle condition

Rather than checking only whether a fault occurs, the test cases verify **when** it occurs, **when** it recovers, and **whether it is erased according to specification**.

---

## Key Test Scenarios

### 1. Battery Percent Low
This scenario validates whether a battery low diagnostic is handled correctly depending on battery percentage.

Main checks include:
- Non-detection check at **16%**
- Detection boundary check at **15%**
- Detection limit check at **0%**
- Non-healing check at **29%**
- Healing boundary check at **30%**
- Healing limit check at **100%**
- DTC erase check after healing state and repeated **IGN Off → On** cycling

### 2. Battery Charging Error
This scenario validates whether a charging-related diagnostic is detected and cleared correctly depending on battery percentage and charging status.

Main checks include:
- Non-detection check at **79%** with charging status fault input
- Detection boundary check at **80%**
- Detection limit check at **100%**
- Non-healing check at **79%** after fault recovery input
- Healing boundary check at **80%**
- Healing limit check at **100%**
- DTC erase check after healing state and repeated **IGN Off → On** cycling

---

## Test Architecture
The test environment is structured to simulate external ECU inputs and validate diagnostic outputs in a black-box manner.

### Input simulation modules
The following CAPL modules generate cyclic CAN messages and update signal values through system variables:

- `BMS.can` : battery-related inputs such as battery percent, charge status, and voltage
- `BRK.can` : brake, accelerator, and vehicle speed inputs
- `ENG.can` : ignition and engine state inputs
- `STR.can` : steering angle input
- `UDS.can` : diagnostic request messages such as fault status request and fault clear request

### Test execution module
- `tfs_IVS_Project.can`
  - contains the main CAPL-based automated test logic
  - defines helper functions such as DTC clear and fault status request
  - executes timing-based validation for detection, healing, and erase requirements

### CANoe project configuration
- `IVS_Project.tse`
- `IVS_Project.sttse`

These files contain the CANoe test setup and execution environment.

---

## Repository Structure
```text
IVS_BlackBoxTest/
├─ BMS.can
├─ BRK.can
├─ ENG.can
├─ STR.can
├─ UDS.can
├─ tfs_IVS_Project.can
├─ IVS_Project.tse
├─ IVS_Project.sttse
├─ IVS_version_1.canencr
└─ tfs_IVS_Project_report.vtestreport

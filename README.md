# BK-Precision-1900-Series-LabVIEW-USB-Control-SubVI
This repository contains a LabVIEW subVI for controlling BK Precision 1900 Series DC power supplies via USB (VISA).
The subVI sends voltage and current setpoints to the power supply and reads back the actual measured output voltage and current. It is intended to be used as a reusable subVI inside any LabVIEW application.

## 🔌 Supported Hardware
- BK Precision 1900 Series DC Power Supplies
  - 1900 / 1901 / 1902 / 1903 (and compatible models)
- Communication:
  - USB (NI-VISA)

## 📌 Key Features
- USB-based VISA communication
- Designed as a drop-in subVI
- Sends:
  - Voltage set value
  - Current limit set value
- Reads back:
  - Actual output voltage
  - Actual output current
- Internal timing and sequencing handled automatically
- Returns scaled values in volts and amps
- VISA error propagation

## 🧠 What the SubVI Does Internally
Each time the subVI is called, it performs the following steps:
1. Sends voltage set command
2. Sends current limit set command
3. Queries actual output voltage
4. Queries actual output current
5. Reads and parses USB responses
6. Applies scaling and outputs numeric values
The calling VI does not need to manage USB timing, delays, or byte counting.

## 🧩 SubVI Inputs & Outputs
### Inputs
- VISA Resource Name (USB)
- Voltage Set Value (V)
- Current Set Value (A)
- Error In
### Outputs
- Measured Voltage (V)
- Measured Current (A)
- Error Out

## ▶️ How to Use This SubVI in Your Own VI
This subVI is meant to be called from any LabVIEW VI (test sequence, state machine, loop, etc.).
### Step 1: Add the SubVI to Your Block Diagram
- Copy the subVI into your project or a shared library folder
- Drag and drop it onto the block diagram of your VI
### Step 2: Open and Configure VISA (Recommended)
For best performance and clean architecture:
- Open the USB connection once using VISA Open
- Pass the VISA reference to this subVI
- Close VISA when your application finishes

`⚠️ Avoid opening and closing VISA repeatedly inside loops.`
### Step 3: Wire the Inputs
Provide:
- Desired voltage setpoint
- Desired current limit
- VISA reference
- Error wire (recommended for proper execution order)

Example usage:
- Inside a While Loop for continuous control
- Inside a State Machine for step-based testing
- Inside a For Loop for automated sweeps

### Step 4: Read the Outputs
Use the returned values to:
- Display actual voltage/current
- Log measurements to file
- Implement protection logic
- Perform pass/fail testing
- Close the control loop
The outputs always represent the actual measured values, not just the setpoints.

### Step 5: Handle Errors
- Monitor the Error Out terminal
- Stop execution or take corrective action if VISA errors occur

### 🔁 Typical Integration Patterns
<img width="3079" height="781" alt="BK1900B Driver" src="https://github.com/user-attachments/assets/1c593315-f947-4f7a-b365-5f05c59a57f6" />
Common ways to integrate this subVI:
- Single-shot control
  - Set voltage/current once and read back values
- Continuous monitoring
  - Call inside a While Loop
- Automated test sequence
  - Call from a state machine or queued message handler
- Power sweep
  - Step voltage/current across ranges and log results

### 📐 Data Scaling
- Raw USB responses from the power supply are converted internally
- Caller receives values directly in:
  - Volts (V)
  - Amps (A)
No additional conversion is required.

### 🛠️ Requirements
- LabVIEW 2018 or newer
- NI-VISA installed
- BK Precision USB driver
- BK Precision 1900 Series power supply

### ⚠️ Notes & Best Practices
- Ensure the power supply is in remote mode if required
- Verify the correct USB VISA resource name
- Keep the subVI call rate reasonable (avoid extremely fast polling)
- Use error wiring to enforce execution order

### 🔧 Possible Enhancements
- Output ON/OFF control
- Timeout configuration
- Multi-instrument support

### 📚 References
[1900B_Series_programming_manual.pdf](https://github.com/user-attachments/files/24417476/1900B_Series_programming_manual.pdf)

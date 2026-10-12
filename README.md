# LimaCharlie EDR Setup & Telemetry Analysis Lab

## Objective
This project demonstrates the deployment, configuration, and telemetry validation of the **LimaCharlie Endpoint Detection and Response (EDR)** agent on a Windows host. The goal is to capture live process creation telemetry and construct custom Detection & Response (D&R) rules for Threat Hunting and SOC monitoring.

## System Architecture & Details

![Architecture Diagram](assets/architecture-diagram png)

* **EDR Agent:** LimaCharlie Sensor v5.3.12 (64-bit)
* **Endpoint Host:** Windows 10 (`desktop-1q7qks0.lan`)
* **Deployment Method:** Administrative PowerShell CLI
* **SIEM / Telemetry Transport:** Real-time SaaS Cloud Streaming

---

## Lab Execution & Implementation

### 1. Agent Deployment & Service Verification
The LimaCharlie sensor was installed via PowerShell running as Administrator using the organization installation key. Verification was confirmed by inspecting the running service state.

![Agent Installation & Service Verification](assets/1-agent-installation png)

### 2. Sensor Cloud Enrollment & Telemetry Streaming
Once installed, the endpoint connected to the LimaCharlie cloud console. Live telemetry streaming was validated by checking the active sensor connection status and real-time process list.

![Sensor Cloud Enrollment](assets/2-cloud-enrollment png)

### 3. Process Creation Telemetry Analysis
Process execution telemetry (`NEW_PROCESS` events) was captured and analyzed. Telemetry fields were inspected to extract process paths, parent PIDs, command-line parameters, and `CODE_IDENTITY` hashes.

![Process Telemetry Event Analysis](assets/3-process-telemetry png)

### 4. Detection & Response (D&R) Rule Construction
A custom D&R rule was engineered in YAML to detect unapproved binary executions (`notepad.exe`) and trigger automated alerting actions.

![D&R Rule Engineering](assets/4-dnr-rule-editor png)

### 5. Detection Rule Validation & Alert Triggering
To validate the rule, `notepad.exe` was launched on the Windows endpoint. The LimaCharlie detection engine successfully matched the process creation telemetry and triggered a real-time detection alert.

![Detection Alert Validation](assets/5-detection-alert png)

---

## Repository Structure
* `assets/`: Contains architecture diagrams and step-by-step proof screenshots.
* `rules/`: Custom YAML Detection & Response rules.
* `telemetry/`: Raw JSON event logs captured during testing.

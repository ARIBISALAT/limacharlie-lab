# LimaCharlie EDR Setup & Telemetry Analysis Lab

## Objective
This project demonstrates the deployment, configuration, and telemetry validation of the **LimaCharlie Endpoint Detection and Response (EDR)** agent on a Windows host. The goal is to capture live process creation telemetry, emulate adversary behavior via Atomic Red Team, and construct custom Detection & Response (D&R) rules for Threat Hunting and SOC monitoring.

---

## System Architecture & Details

+---------------------------------+                     +------------------------+                     +-----------------------+                     +-------------------+
| Windows 10 Workstation          |                     |                        |                     |                       |                     | SOC Analyst /     |
| (desktop-1q7qks0.lan)           |---Process Telemetry-->| LimaCharlie EDR Cloud  |---D&R Rules & Alerts->| Microsoft Sentinel    |---Merged PR Rules-->| GitHub Repo       |
| Running rphcpsvc service        |                     |                        |                     | (KQL Analytics)       |                     | (limacharlie-lab) |
+---------------------------------+                     +------------------------+                     +-----------------------+                     +-------------------+


* **Cloud Infrastructure:** Azure Resource Group `rg-livefire-soc` (`vm-win-01`, `vm-linux-01`)
* **SIEM / Analytics:** Microsoft Sentinel (KQL)
* **EDR Agent:** LimaCharlie EDR (`rphcpsvc` Windows service)
* **Adversary Emulation:** Atomic Red Team (`T1087.001`, `T1547.001`)
* **Repository:** `ARIBISALAT/limacharlie-lab`

---

## 🛡️ Proof Artifacts & Implementation Details

### 1. Detection Rule CI/CD Workflow
Authored custom KQL detection rules for MITRE ATT&CK techniques T1087.001 (Local Account Discovery) and T1547.001 (Registry Run Keys Persistence). Submitted changes via a feature branch and merged Pull Request #1 into `main`[cite: 3].

![GitHub PR Merged](docs/images/01_github_pr_merged_kql_rules.png)

### 2. Endpoint Detection & Response (EDR) Verification
Deployed and registered the LimaCharlie EDR sensor on `vm-win-01`. Sensor status verified active (`is_online is true`) on host `desktop-1q7qks0.lan`[cite: 2].

![LimaCharlie Dashboard](docs/images/02_limacharlie_edr_sensor_online.png)

### 3. Local Endpoint Execution & Telemetry
Verified agent operational state at the endpoint layer via PowerShell (`rphcpsvc` service running) while executing Atomic Red Team technique simulations.

![PowerShell Service Status](docs/images/03_powershell_atomic_red_team_execution.png)

### 4. Infrastructure Lifecycle & Cost Control
Concluded simulation lifecycle by deallocating compute resources via Azure CLI (`az vm deallocate`) to eliminate unnecessary subscription consumption[cite: 1].

![Azure VMs Deallocated](docs/images/04_azure_vms_deallocated.png)

---

## 🔍 Key Findings & Triage Notes
* **SIEM Logging Dependency:** Microsoft Sentinel requires an explicit Data Collection Rule (DCR) configured on the Azure Monitor Agent (AMA) to ingest Windows `SecurityEvent` Event ID 4688 (Process Creation) logs into the Workspace.
* **EDR Redundancy:** Continuous process lineage and command-line execution arguments were captured independently at the EDR layer via LimaCharlie, ensuring uninterrupted threat visibility even when SIEM log forwarders experience configuration gaps.

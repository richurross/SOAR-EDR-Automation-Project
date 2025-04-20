# SOAR-EDR-Automation-Project

![image_alt](https://github.com/richurross/SOAR-EDR-Automation-Project/blob/13d414014bba97a2cbaf10a47692874c61f3615b/Playbook%20Flow.gif)

# Project Overview
This lab project demonstrates the integration of SOAR (Tines) and EDR (LimaCharlie) to automate threat detection and response, specifically targeting credential theft attacks using LaZagne. When LimaCharlie detects LaZagne execution on an endpoint, Tines triggers real-time alerts via Slack/email and initiates a user-approved isolation workflow, enabling dynamic decision-making in incident response. The system streamlines security operations by combining automated remediation with human oversight, improving both speed and adaptability in handling compromises.

# Tools Used
- Windows Machine: Target machine for which LimaCharlie will detect for isolation prompt, where users may decide to isolate based on the threat severity.
- LimaCharlie (EDR): Open-source EDR tool used to detect and respond to security incidents and threats.
- Tines (SOAR): SOAR Tool used to automate security workflows and orchestrates the response actions.
- Slack: Acts as the communication platform to receive alert detection messages.
- SquareX (Email): Disposable email webapp used to receive detection our alert emails.

# Insights

- Integrated SOAR (Tines) with EDR (LimaCharlie) to automate end-to-end threat response, reducing simulated attack remediation time by 90% through real-time detection and user-approved isolation workflows.

- Designed and implemented automated security playbooks that streamlined alert triage, enabling <1-minute notification of credential theft (LaZagne) via Slack/email with zero false positives.

- Configured dynamic threat response rules in LimaCharlie to detect endpoint compromises, with 100% success rate in lab-tested attack scenarios and optional isolation prompts for analysts.

- Built interactive SOAR workflows with user decision points (Tines), allowing security teams to approve/deny remediation actions in 2 clicks, balancing automation with human oversight.

- Orchestrated cross-platform incident response by linking EDR, SOAR, and communication tools (Slack/email), ensuring seamless coordination during simulated security events.

Note: Any cloud provider or hosted hypervisor can be utilized for this project, but I used my own personal device.

# LimaCharlie (EDR) Installation

# Slack & Tines Setup

# Tines Workflow Build



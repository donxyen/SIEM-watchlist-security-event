# Microsoft Azure Sentinel Security Event Watchlist 

## OBJECTIVE

A lightweight Azure SIEM/Sentinel proof-of-concept: Azure VM → central Log Analytics workspace → Azure Sentinel with a watchlist-driven KQL investigation that geo-maps attacker IPs from failed logons.

<img width="1370" height="493" alt="5" src="https://github.com/user-attachments/assets/8105f7f1-206e-4ea2-b0c6-5204b9b62e46" />


## ARCHITECTURE

Azure tenant & subscription — root environment for everything.

Resource Group — houses all resources for the demo.

Virtual Network + Subnet — network layer where the VM resides.

Virtual Machine — target host instrumented for telemetry.

VM NSG / firewall intentionally left wide open to simulate exposed service.

VM host firewall also open (simulates a high-risk target).

Log Analytics workspace — central log repository for security data.

Azure Monitor Agent (on the VM) — forwards security logs to Log Analytics.

Azure Sentinel — connected to the Log Analytics workspace to ingest logs and run detections.

Watchlist — uploaded spreadsheet of geographic IP block metadata used for enrichment and lookup.

KQL queries & dashboard — queries correlate failed logons with attacker IPs and watchlist metadata; results are plotted on a map visualization.

## STEPS

### Create Virtual Workspace in Microsoft Azure

<img width="1057" height="493" alt="1" src="https://github.com/user-attachments/assets/81f26104-0a45-4186-a9ea-05f810a37aea" />

- Provisioned network, subnet, and an intentionally exposed VM inside a Resource Group.
- Enabled log collection from the VM by installing/configuring the Azure Monitoring (Log Analytics) agent.
- Created a Log Analytics workspace as a central log repository.
- Connected Azure Sentinel to the workspace.

### Turn Off Firewall
<img width="1035" height="773" alt="2" src="https://github.com/user-attachments/assets/ed985e00-9aec-4cb2-9c38-027f0efc5e40" />

- Remote access to login and turn off firewall for attackers

### Attack Map Simulation
<img width="1311" height="693" alt="4" src="https://github.com/user-attachments/assets/39057b25-f36e-4d8e-bf5f-a66c1ad977aa" />

- Wrote KQL queries that:.
  - Queried failed logon/authentication events from the VM logs.
  - Joined attacker IPs against the watchlist IP blocks.
  - Produced geo coordinates (or country metadata) to determine attack origin.
- Built a Sentinel dashboard that maps the attack sources (visualization shown in demo).
- Demonstrated end-to-end workflow: exposed VM → logs → Sentinel → geo-mapping of attacker IPs.

## RESULTS
- Working pipeline from VM security logs → central Log Analytics → Sentinel ingestion.
- Enrichment using a custom watchlist to map attacker IPs to geographic metadata.
- Ability to correlate failed logons and plot attacker origins on a map — useful for early threat hunt visualizations.
- This is a proof-of-concept SIM/SIEM — not yet a full SOC.



# Splunk SOC Analyst Lab: Brute-Force Detection

## Objective
The goal of this project was to set up a local Security Information and Event Management (SIEM) environment using Splunk, ingest authentic Windows Event logs, and perform threat hunting to detect a Brute-Force attack. Finally, a custom alert was created to automate the detection of this specific threat.

## Tools & Environment
* **SIEM:** Splunk Enterprise
* **OS:** Windows 10
* **Log Source:** EVTX Attack Samples (Event ID 4625 - Failed Logon)

## Step 1: Data Ingestion
I downloaded authentic Windows security logs containing malicious activity (Brute-Force attempts) and successfully ingested the `.evtx` file into Splunk to create a realistic SOC analysis environment.

<img src="1-data ingestion.png">

## Step 2: Threat Hunting with SPL
Using Splunk Processing Language (SPL), I filtered the raw data to isolate failed logon attempts. 
* **Query used:** `index="main" EventCode=4625`

Through the analysis of the logs, I identified the targeted account and determined the attack vector:
* **Target Account Name:** `IEUser`
* **Source Network Address:** `-` (Indicates a local attack originating from the host machine itself, rather than an external IP).

<img src="2 spl-search.png.png">

## Step 3: Custom Rule Creation
To transition from manual hunting to automated detection, I wrote a custom SPL rule designed to trigger when multiple failed logon attempts occur on the same host, indicating a Brute-Force attack.
* **Rule Query:** `index="main" EventCode=4625 | stats count by host | where count >= 1`

<img src="3 Custom rule.png">

## Step 4: Alert Configuration
Finally, I configured Splunk to generate a High-Severity alert based on the custom rule. The system is now automated to detect and report repeated failed logon attempts.
* **Alert Title:** High Severity: Multiple Failed Logon Attempts

<img src="4 Alert.png">

## Conclusion
This lab demonstrates practical, hands-on experience with Splunk, covering the entire lifecycle of a SOC analyst's daily tasks: from data ingestion and SPL querying to log analysis and automated threat detection.

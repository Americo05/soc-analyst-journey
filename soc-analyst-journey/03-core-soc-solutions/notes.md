# 03 – Core SOC Solutions
 
**Status:** ✅ Done · **Completed:** 2026/09
 
**Rooms:** Introduction to EDR · Introduction to SIEM · Splunk: The Basics · Elastic Stack: The Basics · Introduction to SOAR
 
## 🎯 Why this matters in a SOC
These are the tools an L1 analyst uses every day. The **EDR** watches what happens on each endpoint, the **SIEM** brings logs from the whole environment into one place and raises alerts, and the **SOAR** automates the repetitive steps. Knowing what each one sees, and what it can't see, is what lets you investigate an alert instead of just reading it.
 
## 📚 Key Concepts
 
### EDR (Endpoint Detection and Response)
- An agent installed on each endpoint (laptops, servers) that records detailed **telemetry**: process creation, parent/child process relationships, file changes, registry changes, network connections, logins.
- **Detection:** goes beyond traditional antivirus. Instead of only matching known signatures, it also looks at **behaviour** (e.g. Word spawning PowerShell, which is rarely legitimate).
- **Response:** lets the analyst act directly on the endpoint, e.g. **isolate the host** from the network, kill a process, quarantine a file.
- **Process tree:** one of the most useful views in an EDR. Seeing *what launched what* often tells the whole story of an attack.
- **Limitation:** only sees endpoints where the agent is installed. Network devices, cloud services and unmanaged machines are blind spots.
### SIEM (Security Information and Event Management)
- **Collects** logs from many sources: endpoints, servers, firewalls, proxies, email gateways, cloud, authentication systems.
- **Normalises** them into a common format so events from different sources can be searched together.
- **Correlates** events using detection rules, e.g. "10 failed logins followed by a success from the same IP within 5 minutes".
- **Alerts** when a rule matches, and provides dashboards and search for investigation.
- **Garbage in, garbage out:** a SIEM is only as good as the logs it receives and the rules it runs. Missing log sources mean missing detections.
### Splunk
- **Components:**
  - **Forwarder:** lightweight agent that sends logs from the source to Splunk.
  - **Indexer:** receives, parses and stores the data so it can be searched.
  - **Search Head:** the interface where analysts run searches and build dashboards.
- Data is organised in **indexes** and identified by **sourcetype** (the type/format of the log).
- Searches use **SPL** (Search Processing Language), chaining commands with a pipe `|`.
### Elastic Stack (ELK)
- **Elasticsearch:** stores and indexes the data; the search engine.
- **Logstash:** collects, parses and transforms logs before storing them.
- **Kibana:** web interface for searching, visualising and building dashboards.
- **Beats:** lightweight shippers installed on endpoints (e.g. Winlogbeat, Filebeat) that send data to Logstash or Elasticsearch.
- Searches in Kibana's **Discover** tab usually use **KQL** (Kibana Query Language).
### SOAR (Security Orchestration, Automation and Response)
- **Orchestration:** connects the different security tools (SIEM, EDR, email, threat intel, ticketing) so they work together.
- **Automation:** runs repetitive steps automatically, e.g. enriching every IP in an alert with threat intel, or pulling the email headers of a reported phishing message.
- **Response:** executes actions through **playbooks**, e.g. block a domain, disable a user, isolate a host, sometimes after analyst approval.
- **Why it matters for L1:** less time on copy-paste enrichment, more time on actual analysis. It also helps fight alert fatigue.
### How they fit together
```
Endpoints ──(EDR telemetry)──┐
Network / Firewall / Proxy ──┼──► SIEM ──(alert)──► L1 analyst ──► SOAR playbook
Email / Cloud / Auth logs ───┘     correlates          triages         automates enrichment
                                   and alerts                          and response
```
 
## 🧰 Tools
| Tool | What I used it for |
|---|---|
| EDR console | Reviewing endpoint alerts, process trees and response actions |
| Splunk | Searching and filtering logs with SPL, building simple stats |
| Kibana (Elastic) | Searching logs with KQL in Discover, viewing visualisations |
| SOAR platform | Understanding how playbooks automate enrichment and response |
 
## 🔍 What I'd look for in a real alert
- **Which tool raised it?** An EDR alert gives process-level detail; a SIEM alert may need me to pivot into other log sources.
- **Process tree:** is the parent process normal? (e.g. `winword.exe → powershell.exe` is a red flag)
- **Timeline:** what happened on the host right before and right after the alert?
- **Scope:** search the SIEM for the same IoC (IP, domain, hash, user) on other hosts. Is this one machine or many?
- **Blind spots:** is a log source missing that would confirm or rule out the activity?
## 🗺️ MITRE ATT&CK
This section is about tooling rather than specific techniques. One example of what these tools help detect:
 
| Technique | ID | How it shows up in logs |
|---|---|---|
| Command and Scripting Interpreter: PowerShell | T1059.001 | EDR process tree (unusual parent), Windows/PowerShell logs in the SIEM |
 
## ⌨️ Useful Commands / Queries
 
**Splunk (SPL)**
```
# Failed Windows logons, counted per account
index=main EventCode=4625 | stats count by Account_Name | sort -count
 
# Top 10 source IPs
index=main | top limit=10 src_ip
 
# Show only selected fields
index=main sourcetype=WinEventLog | table _time, host, EventCode, Account_Name
 
# Remove duplicates
index=main | dedup src_ip
 
# Least common values (useful to spot anomalies)
index=main | rare limit=10 process_name
```
 
**Kibana (KQL)**
```
# Failed logons
event.code : 4625
 
# Combine conditions
event.code : 4625 and source.ip : "10.10.10.5"
 
# Exclude noise
event.code : 4688 and not user.name : "SYSTEM"
 
# Wildcards
process.name : powershell*
```
 
## 💡 Takeaways
I liked learning about EDR and SIEM because they complement each other: the EDR shows in detail what happens on each endpoint, while the SIEM gives the full picture across the environment. Add SOAR on top, and a lot of repetitive work gets automated, leaving more time for actual investigation.
I also enjoyed Splunk, as it makes logs much easier to understand and correlate.

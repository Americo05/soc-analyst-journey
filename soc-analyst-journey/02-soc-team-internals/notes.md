# 02 – SOC Team Internals
 
**Status:** ✅ Done · **Completed:** 2026-09
 
**Rooms:** SOC L1 Alert Triage · SOC L1 Alert Reporting · SOC Workbooks and Lookups · SOC Metrics and Objectives
**SOC Simulator:** Introduction to Phishing
 
## 🎯 Why this matters in a SOC
Section 01 explained what an L1 analyst does. This one explains how the work actually runs day to day: picking which alert to handle first, investigating it in a consistent way, writing it up so L2 can act on it, and measuring whether the team is doing a good job. It's the part of the job that turns technical knowledge into a repeatable process.
 
## 📚 Key Concepts
 
### Alert triage
- **Anatomy of an alert:** rule/alert name, severity, timestamp, source (SIEM, EDR, email gateway…), affected user and host, and the raw event that triggered it.
- **Prioritisation:** handle the highest severity first; within the same severity, the oldest alert first. An alert left waiting is time the attacker gets for free.
- **Ownership:** assign the alert to yourself before starting, so two analysts don't work the same case and nothing falls through the cracks.
- **Verdict:**
  - **True positive:** real malicious activity → escalate or respond.
  - **False positive:** the rule fired but nothing malicious happened → close with a clear justification.
- **Context is everything:** the same event (e.g. an admin running PowerShell) can be normal for one user and highly suspicious for another.
### Alert reporting and escalation
- A good report lets someone else understand the case **without redoing the investigation**.
- Answer the **5 Ws**: *Who* (user/host), *What* (activity observed), *When* (timeline), *Where* (systems, IPs, domains), *Why* (why it's malicious or benign).
- Include **evidence**: log extracts, IoCs (IPs, domains, hashes), screenshots.
- **Escalate** when the threat is confirmed, the impact is unclear, or the next step is beyond L1's permissions (e.g. isolating a server). When in doubt, escalate with context rather than close.
- Closing a false positive also needs a short justification. It helps tune the rule later.
### Workbooks and lookups
- **Workbook (playbook/runbook):** a documented, step-by-step procedure for a specific alert type, like "suspicious login" or "phishing reported by user". It keeps investigations consistent across analysts and shifts.
- **Lookups / enrichment sources** that give context:
  - **Asset inventory:** what is this host? Who owns it? How critical is it?
  - **Identity inventory:** who is this user? What's their role and normal behaviour?
  - **Network diagram:** how do systems connect? What should talk to what?
  - **Threat intel:** is this IP, domain or hash already known as malicious?
### Metrics and objectives
- **SLA:** the agreed maximum time to acknowledge or resolve an alert, often depending on severity.
- **MTTD** (Mean Time to Detect): how long it takes from the attack starting until it's detected.
- **MTTA** (Mean Time to Acknowledge): how long an alert waits before an analyst picks it up.
- **MTTR** (Mean Time to Respond): how long from detection until the threat is contained.
- **False positive rate:** share of alerts that turn out to be benign. A high rate means noisy rules and analyst fatigue.
- **Escalation rate:** share of alerts L1 escalates. Too high or too low can both mean something is wrong.
- Metrics aren't just for managers: they show where detection or process needs to improve.
## 🧰 Tools
| Tool | What it's for |
|---|---|
| SIEM / alert queue | Where alerts arrive and get assigned |
| Ticketing / case management | Documents each case from triage to closure |
| Workbooks / playbooks | Standard investigation steps per alert type |
| Asset & identity inventories | Context about hosts and users |
| Threat intel lookups | Reputation checks for IPs, domains and hashes |
 
## 🔍 My triage checklist
1. **Assign** the alert to myself.
2. **Understand the rule:** what is it trying to detect?
3. **Gather context:** who is the user, what is the host, is this behaviour normal for them?
4. **Check the evidence:** raw logs, related events before and after, IoC reputation.
5. **Decide:** true positive or false positive?
6. **Act:** close with justification, or escalate with a report that answers the 5 Ws.
## 🗺️ MITRE ATT&CK
This section is about process rather than attacker techniques. The SOC simulator scenario used a phishing case:
 
| Technique | ID | How it shows up in logs |
|---|---|---|
| Phishing | T1566 | Email gateway alerts, user reports, suspicious links or attachments |
 
## ⌨️ Quick Reference
```
Report skeleton
- Summary:     one or two lines, what happened and the verdict
- Who / Where: affected user(s), host(s), IPs, domains
- When:        timeline of key events
- What / Why:  activity observed and why it's malicious or benign
- Evidence:    log extracts, IoCs
- Next steps:  recommended action or reason for closing
 
Metrics
False positive rate = false positives / total alerts
Escalation rate     = escalated alerts / total alerts
```
 
## 💡 Takeaways
The SOC simulator felt amazing. It’s always great to have hands-on experience to reinforce what I’ve learned.
In the metrics room I realised that every metric matters, because each one gives the team a number it can work to improve.

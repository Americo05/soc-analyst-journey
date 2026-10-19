# 01 – Blue Team Introduction

**Status:** ✅ Done · **Completed:** 2026-09

## 🎯 Why this matters in a SOC
Before triaging a single alert, an analyst needs to know two things: where they sit in the team, and where attacks actually come from. This section covers both. Most incidents start either with a person being tricked or a system being exposed, so knowing these entry points tells you what is worth monitoring and why an alert matters.

## 📚 Key Concepts
### The SOC and the L1 role
- **SOC (Security Operations Center):** the team that monitors an organisation's environment, detects threats and coordinates the response. It works through **people, process and technology**: none of the three works on its own.
- **Blue Team vs Red Team:** the blue team defends (monitoring, detection, response); the red team simulates attackers to test those defences. When both work together to improve detection, it's called *purple teaming*.
- **SOC tiers:**
  - **L1:** first line. Receives alerts, triages them, closes false positives and escalates real threats with enough context.
  - **L2:** deeper investigation of escalated incidents, scoping and containment.
  - **L3 / specialists:** threat hunting, detection engineering, malware analysis, incident response.
- **Alert outcomes:** an alert is a **true positive** (real malicious activity) or a **false positive** (the rule fired, but nothing malicious happened). The L1 job is to tell them apart quickly and document why.
- **Basic alert flow:** alert → triage → classify (TP/FP) → close with justification *or* escalate → report.
### Humans as attack vectors
- **Social engineering** exploits trust, urgency, authority and curiosity instead of technical flaws.
- Common forms: **phishing** (email), **spear phishing** (targeted), **vishing** (voice), **smishing** (SMS), **pretexting** (fake scenario), **baiting**, and physical ones like **tailgating**.
- **MFA fatigue:** the attacker already has the password and floods the user with MFA prompts until they approve one.
- Takeaway for detection: the human is often the first step, but the *evidence* shows up in systems (email logs, login events, process creation).
### Systems as attack vectors
- **Attack surface:** everything an attacker can reach, such as exposed services, web apps, VPNs and remote access.
- **Unpatched vulnerabilities:** known flaws (CVEs) in software that wasn't updated.
- **Misconfigurations:** open ports, public storage, excessive permissions.
- **Weak or default credentials:** easy targets for brute force and password spraying.
- **Supply chain:** compromising a trusted vendor or software update to reach its customers.

## 🧰 Tools
Mostly conceptual in this section. These are the tools the rest of the path builds on:
 
| Tool | What it's for |
|---|---|
| SIEM | Centralises logs from many sources and raises alerts from detection rules |
| EDR | Monitors endpoints (processes, files, network) and can isolate a host |
| Ticketing / case management | Tracks each alert from triage to closure |
| Threat intel platforms | Checks if an IP, domain or hash is already known as malicious |

## 🔍 What I'd look for in a real alert
- **Phishing:** lookalike sender domains, urgent or threatening tone, mismatched links, unexpected attachments
- **Account abuse:** many failed logins followed by a success; logins from unusual countries or at odd hours
- **MFA fatigue:** a burst of MFA push requests for the same user in a short time
- **Exposed systems:** exploitation attempts against public-facing apps; logins with default account names (admin, root, guest)
- **Context first:** who is the user/host, is this normal for them, has it happened before?

## 🗺️ MITRE ATT&CK
| Technique | ID | How it shows up in logs |
|---|---|---|
| Phishing | T1566 | Email gateway logs, user reports, suspicious attachments or links |
| Valid Accounts | T1078 | Successful logins that look normal but come from an unusual place or time |
| Brute Force | T1110 | Many failed logins against one or many accounts |
| MFA Request Generation | T1621 | Repeated MFA push requests for a single user |
| Exploit Public-Facing Application | T1190 | Web server / WAF logs with exploit payloads |
| External Remote Services | T1133 | VPN or RDP logins from unexpected sources |
| Supply Chain Compromise | T1195 | Trusted software behaving unexpectedly after an update |

## ⌨️ Useful Commands / Queries
```
Windows Security Event IDs
4624  Successful logon
4625  Failed logon
4740  Account locked out
```

## 💡 Takeaways
At first I thought the SOC L1 role was only about reading tickets, like in IT support. After this first room I realised there’s more to it, such as investigating alerts.

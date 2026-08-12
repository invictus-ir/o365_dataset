# Office 365 Unified Audit Log Dataset (BEC / M365 attack data)

[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-blue.svg)](LICENSE)
![Records](https://img.shields.io/badge/records-9%2C608-informational)
![Format](https://img.shields.io/badge/format-Office%20365%20UAL-blue)
![Stars](https://img.shields.io/github/stars/invictus-ir/o365_dataset?style=social)

**A free, real-world dataset of Microsoft 365 / Office 365 Unified Audit Log (UAL) records capturing live business email compromise (BEC) attacks — for building and testing detections, threat hunting, training, and DFIR research.** Published by [Invictus IR](https://www.invictus-ir.com), specialists in cloud incident response.

> **TL;DR** — Real Office 365 Unified Audit Logs from actual BEC intrusions. Unzip `auditrecords.7z` with [7-Zip](https://www.7-zip.org/), load into your SIEM or notebook, and use it to build detections for inbox-rule fraud, mailbox-permission abuse, and account takeover. Free with attribution (CC BY 4.0).

---

## At a glance

| | |
|---|---|
| **What** | Office 365 / Microsoft 365 **Unified Audit Log** records from real BEC attacks |
| **Contents** | Multiple business email compromise campaigns — inbox-rule manipulation & mailbox-permission abuse |
| **Format** | `auditrecords.7z` (extract with 7-Zip) → JSON/CSV audit records |
| **Use for** | Detection engineering, threat hunting, SIEM/analytics testing, training, security research |
| **License** | [CC BY 4.0](LICENSE) — reuse freely, just credit Invictus IR |

---

## Why this dataset exists

Realistic Microsoft 365 attack data is hard to find — production logs are sensitive, and synthetic data rarely matches how attackers actually behave. This dataset provides **genuine Unified Audit Log records from real business email compromise incidents**, so defenders can develop and validate detections against the real thing instead of theory.

### What is the Office 365 Unified Audit Log?

The **Unified Audit Log (UAL)** is Microsoft 365's central record of user and admin activity — sign-ins, mailbox actions, inbox-rule changes, permission grants, file access, and admin operations across Exchange Online, SharePoint, Teams, and Entra ID. It's the primary evidence source when investigating a Microsoft 365 compromise.

---

## What's in the data

The dataset captures **business email compromise (BEC)** tradecraft as it appears in the UAL, including:

- **Inbox-rule manipulation** — attacker-created forwarding/deletion rules used to intercept and hide correspondence (e.g. invoice-fraud operations running twin inbox rules).
- **Mailbox-permission abuse** — granting a look-alike / typosquat account access to a target mailbox for persistent read.
- The surrounding **sign-in, mail, and admin operations** that let you reconstruct the full sequence of an account takeover.

Each record is a real UAL event with its native fields (operation, workload, user, client IP, timestamp, and operation-specific parameters).

---

## How to use it

**1. Extract** the archive with [7-Zip](https://www.7-zip.org/):

```bash
7z x auditrecords.7z
```

**2. Explore** in Python:

```python
import pandas as pd, json
records = [json.loads(l) for l in open("auditrecords.json")]   # adjust to extracted filename
df = pd.json_normalize(records)
print(df["Operation"].value_counts())     # e.g. New-InboxRule, Set-Mailbox, Add-MailboxPermission
```

**3. Build & test detections** — replay the events through your SIEM (Microsoft Sentinel, Splunk, Elastic) to validate rules for:

- `New-InboxRule` / `Set-InboxRule` with forwarding or delete actions
- `Add-MailboxPermission` / `Add-RecipientPermission` grants
- Suspicious `Set-Mailbox` forwarding parameters (`ForwardingSmtpAddress`, `ForwardTo`)
- Anomalous sign-ins preceding rule/permission changes

**Related Invictus tooling**

- [Microsoft-Extractor-Suite](https://github.com/invictus-ir/Microsoft-Extractor-Suite) — acquire your own M365 & Azure logs (the tool that pulls data like this)
- [Blue-team-app-Office-365-and-Azure](https://github.com/invictus-ir/Blue-team-app-Office-365-and-Azure) — Splunk app for hunting M365/Azure activity
- [IOCs & TTPs](https://github.com/invictus-ir/IOCs) — indicators and ATT&CK mappings from our BEC research

---

## What you can build with it

- **Detection rules** for BEC, inbox-rule fraud, and mailbox-permission abuse (map to MITRE ATT&CK `T1114`, `T1098`, `T1078.004`).
- **Threat-hunting playbooks** grounded in real attacker behavior.
- **Analyst training** and CTF / lab scenarios using authentic M365 telemetry.
- **Research & benchmarking** of M365 detection tooling.

---

## FAQ

**What is this dataset?**
Real Office 365 / Microsoft 365 Unified Audit Log records from live business email compromise attacks, published free for security research and detection development.

**What attacks does it contain?**
Business email compromise — primarily inbox-rule manipulation and mailbox-permission abuse used for invoice fraud and persistent mailbox access.

**Can I use it to test my SIEM detections?**
Yes — that's a primary use case. Replay the events into Sentinel, Splunk, or Elastic and validate your alerting.

**Is it safe / legal to use?**
Yes, for defensive research. It's shared under CC BY 4.0. Do **not** attempt to contact or attack any environment, host, or IP address referenced in the data.

**How do I open it?**
Extract `auditrecords.7z` with [7-Zip](https://www.7-zip.org/).

---

## How to cite

A `CITATION.cff` is included (GitHub shows a **"Cite this repository"** button). Suggested citation:

> Invictus IR. *Office 365 Unified Audit Log Dataset (BEC).* GitHub, https://github.com/invictus-ir/o365_dataset

---

## Disclaimer & license

Provided **as-is for defensive security research and detection development**. The dataset contains real malicious activity — use at your own risk and do not attack any environment or IP address referenced. Feedback is welcome via issues.

**License:** [CC BY 4.0](LICENSE) · **More research:** https://www.invictus-ir.com/news · **Company:** https://www.invictus-ir.com

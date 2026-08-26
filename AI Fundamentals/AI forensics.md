__The AI forensics landscape__

*data processing:* manual and tedious task is now parallelised, automated using transformer models etc.
*anomaly detection:* large amounts of data are being examined to find any anomolies in the data.


*AI systems correlate time-sequenced data, to reconstruct the timeline of an incident*


*black box AI models:* AI models that take input and produces desired output without evidencing the work.

*federated learning* distributed machine learning where models share a global dataset but keep the raw data produced locally.







********lab********
# DFIR Incident Response Report — Just Another Night at the Office

## 1. Executive Summary

### Organization

**RobbCo**

### Investigation Type

Digital Forensics and Incident Response (DFIR)

### Investigation Objective

Determine whether RobbCo's systems were compromised, identify the initial access vector, reconstruct the attack timeline, determine the extent of attacker access, and identify whether proprietary intellectual property was compromised.

### Executive Finding

The investigation confirmed a successful compromise of RobbCo's environment.

The attacker gained initial access through a **phishing email** sent to employee `j.morgan`. The email contained a malicious OpenDocument Spreadsheet (`invoice_Q1_2075.ods`) that executed shell commands when opened.

The malicious document harvested local information including shell history, active sessions, SSH configuration, SSH key information, and usernames. The collected information was written to `/tmp/invoice_dump.txt` and transmitted to the attacker's infrastructure.

The attacker subsequently authenticated as `j.morgan`, established remote access through a reverse shell, escalated privileges to the `r.house` account by modifying its SSH `authorized_keys`, established persistent privileged access, and ultimately staged RobbCo's proprietary source code for exfiltration.

The investigation also demonstrated that machine-learning-assisted detection can effectively identify suspicious artifacts, but human validation remained necessary. Several legitimate RobbCo source-code files were incorrectly classified as suspicious.

---

# 2. Incident Timeline

| Time / Phase         | Event                                                            | Evidence                                                |
| -------------------- | ---------------------------------------------------------------- | ------------------------------------------------------- |
| Initial Access       | Phishing email sent to `j.morgan`                                | `/home/j.morgan/Mail/inbox/email_invoice.eml`           |
| Initial Access       | Malicious `invoice_Q1_2075.ods` opened                           | `/home/j.morgan/Documents/Invoices/invoice_Q1_2075.ods` |
| Data Collection      | Local shell history, users, SSH configuration and keys collected | `/tmp/invoice_dump.txt`                                 |
| Exfiltration         | Collected information sent to `192.168.0.100:8080/collect.php`   | Malicious document contents                             |
| **03:00:01 Jan 15**  | Failed SSH login for invalid `admin` account                     | `/var/log/auth.log`                                     |
| **03:01:02 Jan 15**  | Successful SSH authentication as `j.morgan`                      | `/var/log/auth.log`                                     |
| Post-Exploitation    | First-stage tooling downloaded                                   | `/tmp/.syncd`                                           |
| Post-Exploitation    | Reverse shell established as `j.morgan`                          | `/tmp/.x`                                               |
| Privilege Escalation | SSH access to `r.house` enabled by modifying `authorized_keys`   | `/home/j.morgan/.bash_history`                          |
| Persistence          | Privileged reverse shell disguised as `sysmon`                   | `/usr/local/bin/sysmon`                                 |
| Defense Evasion      | Fake boot telemetry created                                      | `/opt/robbco/sys/boot_monitor.log`                      |
| Collection / Staging | RobbCo source code compressed and encoded                        | `/dev/shm/.core_dump_2025.tgz.enc`                      |

---

# 3. Initial Access

## 3.1 Phishing Email

The investigation identified a phishing email delivered to `j.morgan`.

**Sender:**

`akeane@poseidonenergy.net`

The message contained the attachment:

`invoice_Q1_2075.ods`

The attachment was presented as an invoice but contained malicious shell commands rather than a legitimate spreadsheet.

## 3.2 Malicious Attachment

The file was located at:

`/home/j.morgan/Documents/Invoices/invoice_Q1_2075.ods`

Although the file used an `.ods` extension, examination identified it as ASCII text containing shell commands.

The payload:

* collected `.bash_history`
* enumerated active users and sessions using `who`
* collected SSH configuration
* listed SSH keys
* enumerated usable system accounts from `/etc/passwd`
* stored the collected information in `/tmp/invoice_dump.txt`
* transmitted the collected data to `192.168.0.100:8080/collect.php`

This indicates that opening the malicious document provided the attacker with reconnaissance information that could be used to obtain valid credentials and identify viable access paths.

---

# 4. Credential Access and Initial Compromise

Analysis of `/var/log/auth.log` using the supplied ML-assisted classification script identified suspicious authentication activity.

Relevant events included:

```text
Jan 15 03:00:01 — Failed password for invalid user admin
Jan 15 03:01:02 — Accepted password for j.morgan
Jan 15 03:15:00 — Accepted publickey for r.house
```

The successful login for `j.morgan` occurred at:

**03:01:02 on January 15**

The sequence of the failed `admin` authentication followed by successful authentication as `j.morgan` is consistent with the attacker using information obtained during the initial phishing stage to gain authenticated access.

---

# 5. Post-Exploitation Tooling

Following successful access to `j.morgan`, the attacker deployed additional tooling.

### `/tmp/.syncd`

The file connected to:

`http://10.0.0.66/payload.sh`

and executed the retrieved payload.

This functioned as a first-stage dropper used to retrieve additional tooling.

### `/tmp/.x`

The file operated as a reverse-shell stub and established a connection to:

`10.0.0.66:4444`

This provided the attacker with interactive remote access under the `j.morgan` user context.

---

# 6. Privilege Escalation

The attacker escalated from `j.morgan` to the highly privileged `r.house` account.

The key evidence was recovered from:

`/home/j.morgan/.bash_history`

The relevant command was:

```bash
sudo nano /home/r.house/.ssh/authorized_keys
```

The attacker abused legitimate `sudo` permissions to modify the SSH `authorized_keys` file belonging to `r.house`.

By planting an SSH public key in this file, the attacker could authenticate as `r.house` without requiring the account's password.

This represents **privilege escalation through abuse of legitimate permissions**, rather than exploitation of a software vulnerability.

---

# 7. Persistence and Defense Evasion

After gaining access to `r.house`, the attacker established a second reverse shell designed to appear legitimate.

## `/usr/local/bin/sysmon`

The binary connected outbound to:

`10.0.0.66:5555`

Although named to resemble a legitimate system monitoring utility, it contained reverse-shell functionality.

## `/opt/robbco/sys/boot_monitor.log`

A fabricated boot telemetry log was also created.

The purpose of this file was to make the presence of `sysmon` appear legitimate and support the deception that it was part of RobbCo's monitoring infrastructure.

The combination of a disguised binary and supporting fake telemetry indicates deliberate **persistence and defense evasion**.

---

# 8. Intellectual Property Theft

The attacker ultimately targeted RobbCo's proprietary source code.

The investigation identified legitimate source-code files including:

* `/opt/robbco/engineering/MFBootAgent/mfboot_main.c`
* `/opt/robbco/firmware/RETROS_BIOS/core.asm`

The ML model flagged these files as suspicious; however, human investigation determined that they were legitimate RobbCo intellectual property rather than malicious files.

This demonstrates an important limitation of automated anomaly detection: an artifact being statistically unusual does not necessarily mean that it is malicious.

## Staged Exfiltration Archive

The key theft artifact was:

```text
/dev/shm/.core_dump_2025.tgz.enc
```

The archive contained stolen RobbCo intellectual property and had been Base64-encoded.

The use of `/dev/shm` provided a temporary shared-memory location suitable for stealthy staging.

The archive could be decoded and extracted using:

```bash
base64 -d /dev/shm/.core_dump_2025.tgz.enc > /tmp/stolen.tar.gz
tar -xzvf /tmp/stolen.tar.gz -C /tmp/stolen_source
```

The investigation therefore confirmed that the attacker progressed beyond system access and persistence to **theft and staging of proprietary source code**.

---

# 9. Attack Chain

The complete attack sequence reconstructed from the available evidence is:

```text
Phishing Email
      ↓
Malicious invoice_Q1_2075.ods
      ↓
Shell commands executed
      ↓
Credential / system reconnaissance
      ↓
Data written to /tmp/invoice_dump.txt
      ↓
Reconnaissance data exfiltrated
      ↓
SSH login as j.morgan
      ↓
/tmp/.syncd downloads additional tooling
      ↓
/tmp/.x establishes reverse shell
      ↓
sudo used to modify r.house authorized_keys
      ↓
SSH access as r.house
      ↓
/usr/local/bin/sysmon provides privileged persistence
      ↓
Fake boot_monitor.log supports deception
      ↓
RobbCo source code collected
      ↓
Source code compressed and Base64-encoded
      ↓
/dev/shm/.core_dump_2025.tgz.enc
```

---

# 10. Key Evidence

| Artifact                                                | Finding                                         | Significance                                |
| ------------------------------------------------------- | ----------------------------------------------- | ------------------------------------------- |
| `/var/log/auth.log`                                     | Successful `j.morgan` login at 03:01:02         | Confirms authenticated access               |
| `/home/j.morgan/Mail/inbox/email_invoice.eml`           | Phishing email from `akeane@poseidonenergy.net` | Identifies initial access vector and sender |
| `/home/j.morgan/Documents/Invoices/invoice_Q1_2075.ods` | Malicious shell payload                         | Initial compromise mechanism                |
| `/tmp/invoice_dump.txt`                                 | Harvested system/user information               | Credential and reconnaissance collection    |
| `/tmp/.syncd`                                           | Downloads `payload.sh`                          | Additional attacker tooling                 |
| `/tmp/.x`                                               | Reverse shell to `10.0.0.66:4444`               | Remote access                               |
| `/home/j.morgan/.bash_history`                          | `sudo nano /home/r.house/.ssh/authorized_keys`  | Privilege escalation                        |
| `/usr/local/bin/sysmon`                                 | Reverse shell to `10.0.0.66:5555`               | Privileged persistence                      |
| `/opt/robbco/sys/boot_monitor.log`                      | Fake telemetry                                  | Defense evasion                             |
| `/dev/shm/.core_dump_2025.tgz.enc`                      | Encoded stolen archive                          | Intellectual-property theft                 |

---

# 11. Investigation Findings

### Confirmed

* RobbCo was compromised.
* Initial access was achieved through phishing.
* The phishing attachment was malicious.
* The attacker successfully authenticated as `j.morgan`.
* Reconnaissance data was collected and exfiltrated.
* The attacker established a reverse shell.
* The attacker escalated privileges to `r.house`.
* Persistence was established using a disguised reverse shell.
* RobbCo proprietary source code was staged for exfiltration.
* The stolen data included source code associated with the MFBoot Agent and RETROS BIOS.
* Machine-learning classification produced useful investigative leads but also generated false positives.

### Primary Attacker Infrastructure Identified

```text
192.168.0.100:8080
10.0.0.66:4444
10.0.0.66:5555
```

---

# 12. Incident Assessment

The incident represents a **successful multi-stage compromise** involving:

* Phishing
* Malicious document execution
* Credential and system reconnaissance
* Valid-account authentication
* Remote access
* Privilege escalation
* SSH key manipulation
* Persistence
* Defense evasion
* Intellectual-property theft
* Data staging and exfiltration

The attacker demonstrated an ability to move from an employee workstation context to a highly privileged account without relying on a conventional software exploit. The use of legitimate permissions and disguised tooling increased the difficulty of detection.

---

# 13. Detection and Response Observations

The ML-assisted tooling was useful for reducing the initial search space by identifying suspicious authentication events and filesystem artifacts.

However, automated classification was not sufficient for final determination.

Notably, the model incorrectly classified legitimate RobbCo source-code files as suspicious. Human investigation was required to distinguish:

**malicious artifacts** from **legitimate but unusual proprietary files**.

This reinforces the role of ML as an investigative aid rather than an autonomous decision-maker in a forensic investigation.

---

# 14. Final Case Summary

The investigation established the following attack narrative:

An attacker sent `j.morgan` a phishing email from `akeane@poseidonenergy.net` containing a malicious invoice disguised as an `.ods` document. When opened, the document harvested local reconnaissance data and transmitted it to attacker infrastructure.

Using the obtained information, the attacker successfully authenticated as `j.morgan` at **03:01:02 on January 15**. Additional tooling provided a reverse shell, after which the attacker abused `sudo` permissions to modify `r.house`'s SSH `authorized_keys`.

The attacker subsequently gained privileged access and deployed a disguised reverse shell as `/usr/local/bin/sysmon`, supported by fabricated telemetry designed to make the binary appear legitimate.

With privileged access established, the attacker collected RobbCo's proprietary source code and staged the stolen material in:

`/dev/shm/.core_dump_2025.tgz.enc`

The investigation therefore confirms both **unauthorized system access and theft of proprietary intellectual property**.

## Final Answers

| Question                     | Finding                                                           |
| ---------------------------- | ----------------------------------------------------------------- |
| Successful `j.morgan` login  | **03:01:02**                                                      |
| Initial access method        | **Phishing**                                                      |
| Attacker email               | **[akeane@poseidonenergy.net](mailto:akeane@poseidonenergy.net)** |
| Privilege escalation command | **`sudo nano /home/r.house/.ssh/authorized_keys`**                |
| Stolen archive               | **`/dev/shm/.core_dump_2025.tgz.enc`**                            |

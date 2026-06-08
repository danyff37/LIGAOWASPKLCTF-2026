# CTF Write-up: Investigation - III

## Challenge Overview

* **Name:** Investigation - III
* **CTF:** LIGA OWASP KL CTF 2026
* **Category:** Forensics / Log Analysis
* **Objective:** Determine the name of the legitimate process that was hijacked by the malicious stager, `phc.exe`, using the target PID (`10424`) identified in the previous phase.

---

## Tools Used

* Linux Terminal (WSL)
* `grep` - For cross-referencing Process IDs and image names within the parsed XML logs

---

## Step-by-Step Solution

### Step 1: Correlating PID to Process Image

In forensic log analysis, identifying the process image associated with a specific PID at a given timestamp is crucial for understanding the impact and scope of a breach. After confirming in Part 2 that PID `10424` was the target for memory injection, the parsed XML logs were queried to map this PID to its corresponding executable.

By searching for network and process events associated with `10424`, the following Sysmon Event ID 22 (DNS Query) entry was isolated:

```xml
<Data Name="ProcessId">10424</Data>
<Data Name="QueryName">owaspkl.3cc83feaa3b37384a190dd84b25a4592.xyz</Data>
<Data Name="Image">C:\Program Files\WindowsApps\Microsoft.MicrosoftOfficeHub_19.2605.59121.0_x64__8wekyb3d8bbwe\M365Copilot.exe</Data>

```

### Step 2: Analysis of the Hijacked Process

The logs indicate that the process `M365Copilot.exe` (PID `10424`) became actively involved in external network communications, resolving a known malicious C2 domain (`owaspkl.3cc83feaa3b37384a190dd84b25a4592.xyz`) immediately following the execution of the stager.

The selection of `M365Copilot.exe` as the injection target is a textbook example of defense evasion and Living-off-the-Land (LotL) techniques:

1. **Trust Exploitation:** `M365Copilot.exe` is a legitimate, digitally signed Microsoft application. By injecting malicious code into this specific process space, the attacker gains the ability to mask their C2 network traffic as legitimate application telemetry or cloud synchronization.
2. **Security Bypass:** Endpoint Detection and Response (EDR) systems and host firewalls often apply lower scrutiny or implicit allow-listing to trusted system applications and native Microsoft binaries compared to unknown executables.
3. **Persistence and Stealth:** Embedding the C2 beacon within an active Microsoft Office component ensures the malicious connection remains active as long as the user's environment is operational, blending in with standard user activity and complicating the remediation process.

---

## Flag

**`OWASPKL{M365Copilot.exe}`**

# CTF Write-up: Investigation - V

## Challenge Overview

* **Name:** Investigation - V
* **CTF:** LIGA OWASP KL CTF 2026
* **Category:** Forensics / Log Analysis
* **Objective:** Identify the absolute file system path of the malicious payload dropped onto the machine by the stager (`phc.exe`).

---

## Tools Used

* Linux Terminal (WSL)
* `grep` - For tracking file creation events within the parsed XML logs

---

## Step-by-Step Solution

### Step 1: Tracking File Creation Events

After identifying the initial stager (`phc.exe`) in Part 1, the forensic investigation must account for any secondary artifacts introduced to the system. Malicious stagers typically download or extract a primary payload—often a dynamic link library (DLL) or raw shellcode—and write it to a directory with high write permissions prior to executing process injection.

In Sysmon telemetry, Event ID 11 (FileCreate) is the primary source for tracking file system write operations. By filtering the parsed logs for events where the `Image` is `phc.exe` and the operation is file creation, the following activity was revealed:

```xml
<EventID>11</EventID>
<Data Name="Image">C:\Users\ligac\Downloads\phc.exe</Data>
<Data Name="TargetFilename">C:\Users\ligac\AppData\Local\Temp\maindll.dll</Data>

```

### Step 2: Payload Analysis

The telemetry confirms that the stager successfully dropped a file named `maindll.dll` into the user's local Temp directory. The significance of this specific location and file is two-fold:

1. **Directory Selection:** The `AppData\Local\Temp` directory is a classic target drop zone for malware. It is globally writable by user-level accounts and is rarely subjected to strict folder-level monitoring by default security policies. This allows the payload to safely reside on disk without triggering immediate privilege escalation alerts.
2. **Payload Functionality:** In this specific infection chain, `phc.exe` acted strictly as the "loader" or "stager," while `maindll.dll` served as the "payload"—the actual malicious code containing the C2 beacon functionality. Once this DLL was written to disk, the stager read it and injected it into the memory space of the target process (`M365Copilot.exe`, identified in Part 3) to establish the active C2 connection.

---

## Flag

**`OWASPKL{C:\Users\ligac\AppData\Local\Temp\maindll.dll}`**

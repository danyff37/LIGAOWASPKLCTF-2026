# CTF Write-up: Investigation - II

## Challenge Overview

* **Name:** Investigation - II
* **CTF:** LIGA OWASP KL CTF 2026
* **Category:** Forensics / Log Analysis
* **Objective:** Identify the specific Process ID (PID) that the threat actor targeted for injection/hijacking using the stager identified in Part 1.

---

## Tools Used

* Linux Terminal (WSL)
* `grep` - For searching execution parameters within the parsed XML logs

---

## Step-by-Step Solution

### Step 1: Analyzing the Stager's Execution Parameters

Following the identification of `phc.exe` as the malicious stager in the previous phase, the analysis shifted to how the binary was executed. In malware forensics, numerical arguments passed to a stager via the command line frequently represent a target Process ID (PID) intended for memory injection.

By re-examining the Sysmon Event ID 1 (Process Creation) logs extracted during Part 1, we can isolate the exact command-line arguments used during the stager's execution:

```xml
<Data Name="Image">C:\Users\ligac\Downloads\phc.exe</Data>
<Data Name="CommandLine">phc.exe  10424</Data>
<Data Name="ParentImage">C:\Windows\System32\cmd.exe</Data>

```

### Step 2: Correlation and Context of Process Injection

The command-line argument explicitly shows `10424` being passed to the executable. In advanced persistent threat (APT) scenarios, this indicates the threat actor instructed the stager to attach to the memory space of an existing, legitimate process running under PID `10424`.

Techniques such as "process hollowing" or "reflective DLL injection" are commonly used by C2 beacons for several tactical advantages:

1. **Evasion:** The malicious network activity and execution appear to originate from a trusted or expected system process, bypassing basic endpoint detection.
2. **Persistence:** The connection to the C2 server remains alive even if the initial stager binary (`phc.exe`) is terminated.
3. **Privilege Escalation:** If the targeted process operates at a higher integrity level, the injected shellcode can leverage those permissions.

The presence of this PID in the command-line argument, corroborated by the subsequent discovery commands (`whoami`, `netstat`, `route`) observed originating from the hijacked process, confirms that `10424` was the exact process targeted and successfully hijacked by the attacker.

---

## Flag

**`OWASPKL{10424}`**

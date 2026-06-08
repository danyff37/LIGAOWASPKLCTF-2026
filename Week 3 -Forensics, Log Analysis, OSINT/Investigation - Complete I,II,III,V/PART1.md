# CTF Write-up: Investigation - I

## Challenge Overview

* **Name:** Investigation - I
* **CTF:** LIGA OWASP KL CTF 2026
* **Category:** Forensics / Log Analysis
* **Objective:** Conduct malware analysis on a provided Windows Event Log to identify the file name of the stager used to drop an active C2 beacon.

---

## Tools Used

* Linux Terminal (WSL)
* `evtx_dump` - A Rust-based parser to convert binary EVTX files into readable XML
* `grep` - For text search, filtering, and pattern matching within the logs

---

## Step-by-Step Solution

### Step 1: Log Parsing and Preparation

Windows Event Log (`.evtx`) files utilize a compressed binary format, which causes standard text extraction tools like `strings` to fail or drop critical context. To accurately review the telemetry, the binary log first needed to be converted into a readable XML format.

We used a fast EVTX parser (`evtx_dump`) to extract the data. First, we downloaded the tool and made it executable, then parsed the provided log file into a clean XML file:

```bash
# Download and make the parser executable
$ wget https://github.com/omerbenamram/evtx/releases/download/v0.8.1/evtx_dump-v0.8.1-x86_64-unknown-linux-gnu -O evtx_dump
$ chmod +x evtx_dump

# Parse the EVTX into clean XML
$ ./evtx_dump dump.evtx > dump_parsed.xml

```

### Step 2: Hunting the Stager

With the data converted to a structured XML format, the next step was filtering the noise. Since the objective was to find a specific executable file, a basic `grep` command was issued to surface all `.exe` strings within the parsed event logs:

```bash
$ grep -i ".exe" dump_parsed.xml

```

### Step 3: Analysis and Identification

The output generated a massive wall of text containing standard Windows background processes (e.g., `svchost.exe`, `msedge.exe`, `MsMpEng.exe`). By manually sifting through the execution events, one specific anomaly stood out:

```xml
<Execution ProcessID="10504" ThreadID="6648"></Execution>
<Data Name="Image">C:\Users\ligac\Downloads\phc.exe</Data>
<Data Name="CommandLine">phc.exe  10424</Data>
<Data Name="ParentImage">C:\Windows\System32\cmd.exe</Data>

```

Several Indicators of Compromise (IoCs) supported this finding:

1. **Suspicious Execution Path:** The file `phc.exe` was executed directly from the user's `Downloads` directory, a classic initial drop zone for payloads.
2. **Process Lineage:** The executable was spawned by the command prompt (`cmd.exe`), indicating script-based or terminal execution rather than a typical user double-click.
3. **Command Line Arguments:** The payload was passed a specific numerical argument (`10424`). In malware staging, this frequently indicates a target Process ID (PID) intended for shellcode/process injection.
4. **Post-Exploitation Activity:** Analyzing the telemetry immediately following the execution of `phc.exe` revealed native Windows binaries like `whoami.exe`, `NETSTAT.EXE`, and `ROUTE.EXE` launching. These are automated situational awareness and discovery commands initiated by a C2 beacon the moment it successfully stages.

*(Note: While another executable, `test.exe`, appeared in the logs, it was logged strictly as a `TargetFilename` being scanned by Windows Defender, lacking the execution chain characteristic of the active threat.)*

Based on the execution path and subsequent discovery commands, `phc.exe` was definitively verified as the initial stager.

---

## Flag

**`OWASPKL{phc.exe}`**


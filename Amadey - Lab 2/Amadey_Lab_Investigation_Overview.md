# Amadey Trojan Stealer - Lab 2: Investigating a Modular Malware Infection

This lab simulates an investigation into a Windows machine infected with the Amadey Trojan Stealer. The investigation was conducted using Volatility 3 to analyze a memory dump. The goals included identifying the rogue process, determining how it communicated with its command and control server, identifying downloaded modules, and locating persistence mechanisms.

**Full Video Walkthrough**: 

[Click here to watch on YouTube](https://www.youtube.com/watch?v=b2Gc33euHG4)

---

## Initial Process Analysis

**Step 1: List Running Processes**

```bash
python3 vol.py -f "Windows 7 x64-Snapshot4.vmem" windows.pslist
```

- A suspicious process named `lssass.exe` was observed with PID `2748`.
- The name mimics the legitimate `lsass.exe`, suggesting masquerading.
- Its Parent PID (PPID: 2524) did not map to a known process in the capture, raising suspicion.

---

## Identifying Process Location

**Step 2: Extract Command Line Arguments**

```bash
python3 vol.py -f "Windows 7 x64-Snapshot4.vmem" windows.cmdline --pid 2748
```

- Command line revealed the binary path:
  ```
  C:\Users\0XSH3R~1\AppData\Local\Temp\925e7e99c5\lssass.exe
  ```
- Located in a Temp directory, commonly used by malware.

---

## Command & Control (C2) Communications

**Step 3: Scan Network Connections**

```bash
python3 vol.py -f "Windows 7 x64-Snapshot4.vmem" windows.netscan | grep 2748
```

- Found connections to `41.75.84.12:80`
- Indicates communication with a likely C2 server

---

## Fetching Additional Modules

**Step 4: Analyze Memory for URLs**

Malfind and dlllist produced no leads, so memory was dumped instead:

```bash
python3 vol.py -f "Windows 7 x64-Snapshot4.vmem" windows.memmap --pid 2748 --dump
strings pid.2748.dmp | grep "GET /"
```

- Found HTTP GET requests:
  ```
  GET /rock/Plugins/cred64.dll HTTP/1.1
  GET /rock/Plugins/clip64.dll HTTP/1.1
  ```

- Suggests malware downloads two DLLs for expanded capabilities

---

## Locating Retrieved DLL Files

**Step 5: Scan for File Artifacts**

```bash
python3 vol.py -f "Windows 7 x64-Snapshot4.vmem" windows.filescan | grep -E "cred64|clip64"
```

- Found:
  ```
  \Users\0xSh3rl0ck\AppData\Roaming\116711e5a2ab05\clip64.dll
  ```
- Confirmed one DLL location (cred64.dll did not appear in scan, but was seen in memory)

---

## DLL Execution Method

**Step 6: Identify Child Process**

- `pslist` showed `rundll32.exe` was spawned by `lssass.exe`
- Rundll32 is a trusted Windows utility often used to execute DLLs

---

## Persistence Mechanism

**Step 7: Investigate Task Scheduler Entries**

```bash
python3 vol.py -f "Windows 7 x64-Snapshot4.vmem" windows.filescan | grep -E "lssass"
```

- Found:
  ```
  \Users\0XSH3R~1\AppData\Local\Temp\925e7e99c5\lssass.exe
  \Windows\System32\Tasks\lssass.exe
  ```

- The second path points to Task Scheduler persistence (Scheduled Task)

---

## Summary of Findings

| Key Aspect            | Details                                                                 |
|------------------------|-------------------------------------------------------------------------|
| Suspicious Process     | `lssass.exe` (PID 2748)                                                  |
| File Location          | `C:\Users\0XSH3R~1\AppData\Local\Temp\925e7e99c5\lssass.exe`             |
| C2 IP Address          | `41.75.84.12`                                                            |
| Downloaded DLLs        | `cred64.dll`, `clip64.dll`                                               |
| DLL Location Found     | `AppData\Roaming\116711e5a2ab05\clip64.dll`                              |
| Execution Vector       | `rundll32.exe` (spawned by `lssass.exe`)                                |
| Persistence Mechanism  | Scheduled Task at `\Windows\System32\Tasks\lssass.exe`                   |

---

## Tools Used

- `windows.pslist` – Enumerate running processes
- `windows.cmdline` – Get process command-line arguments
- `windows.netscan` – Identify network connections
- `windows.memmap` – Dump memory regions
- `strings` – Search for URLs in dumped memory
- `windows.filescan` – Locate file artifacts in memory

---

## Notes

This lab required pivoting beyond surface-level plugins. `malfind` and `dlllist` returned no actionable results, so a memory dump (`memmap`) combined with `strings` analysis led to discovering the downloaded payloads. This lateral approach demonstrates real-world adaptability when malware hides in memory or evades simpler plugin detection.

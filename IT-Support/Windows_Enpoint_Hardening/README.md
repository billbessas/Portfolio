# Windows Endpoint Hardening Lab (IT Support)

This project demonstrates foundational endpoint security tasks performed in Windows 11 Pro — including account management, Group Policy configuration, and system auditing — all from the perspective of a desktop support or help desk technician.

The objective was to harden a fresh Windows 11 install against common misconfigurations and initial-access threats, using local tools such as PowerShell, `gpedit.msc`, and Control Panel. The work performed here reflects real-world practices in environments without domain-level controls, such as SMBs or isolated systems.

> **Use Case:** A typical Tier 1–2 IT Support technician preparing a new machine for deployment or cleaning up a compromised user environment.

---

## Lab Objectives

- Apply CIS-aligned system hardening to a standalone Windows 11 Pro system
- Enforce strong password policy and user privilege separation
- Enable local auditing for key security-relevant events
- Simulate and block a basic PowerShell-based payload
- Document changes with CLI commands, screenshots, and rationale

---

## Highlights

- Renamed the default `Administrator` account  
- Disabled Guest login and created a standard (non-admin) user  
- Enforced minimum password length and complexity  
- Enabled audit logging for login attempts, privilege use, and object access  
- Raised User Account Control (UAC) to highest level  
- Simulated execution of a suspicious PowerShell script before and after hardening

---

## Tools Used

- Windows 11 Pro ARM (in UTM)
- PowerShell
- Local Group Policy Editor (`gpedit.msc`)
- Windows Event Viewer
- Screenshot documentation

---

## Framework Alignment

This lab reflects core elements of:
- **CIS Microsoft Windows 11 Benchmark – Level 1**
- **NIST 800-53 Rev 5**, including:
  - `AC-2` (Account Management)
  - `AC-6` (Least Privilege)
  - `AU-2` (Audit Events)
  - `IA-5` (Authenticator Management)

---

## Full Documentation

See the full hardening walkthrough here:  
[Hardening_Report.md](/Hardening_Report.md)


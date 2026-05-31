# Ubuntu 25.10 sudo Recovery Case Study
This repository documents the investigation and recovery of a broken Ubuntu 25.10 package-management installation where the `sudo` command was unavailable.
The issue was traced to an interrupted package installation that left the `sudo` package in an unpacked but unconfigured state (`iU`).


## Skills Demonstrated
- Linux Administration
- Ubuntu Package Management
- APT Troubleshooting
- DPKG Troubleshooting
- Privilege Escalation Diagnostics
- Root Cause Analysis
- Incident Documentation
- Validation Testing


## Environment
| Component | Value |
|------------|--------|
| OS | Ubuntu 25.10 |
| Platform | VirtualBox VM |
| User | osboxes |
| Package Manager | apt / dpkg |

## Incident Summary

### Symptom
```bash
sudo apt update
Command 'sudo' not found


## Investigation
| Step | Finding |
|--------|----------|
| User verification | Account in sudo group |
| Binary check | /usr/bin/sudo missing |
| Package check | sudo status = iU |
| Package contents | sudo.ws present |
| Privilege escalation | pkexec broken |
| Root cause | Interrupted package configuration |

## Root Cause
The sudo package had been unpacked but never configured.

## Evidence:
- Package state `iU`
- Missing `/usr/bin/sudo`
- Presence of `/usr/bin/sudo.ws`
- Incorrect SUID permissions on pkexec

This indicates an interrupted APT/DPKG transaction.

## Remediation
Validated temporary sudo binary:
/usr/bin/sudo.ws -V

Repaired package database:
/usr/bin/sudo.ws dpkg --configure -a
/usr/bin/sudo.ws apt --fix-broken install

### Validation
sudo whoami

Verify package health:
dpkg --audit

## Lessons Learned
- Understand DPKG package states.
- Investigate temporary package binaries.
- Validate privilege escalation mechanisms.
- Use structured troubleshooting methodology.
- Verify remediation through testing.

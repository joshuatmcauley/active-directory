# active-directory

# AD lab (Hyper-V)

Home lab, not production. Isolated Hyper-V internal switch. No real user data, no domain admin passwords in this repo.

## What this is

A small office-style lab on a Windows 11 Pro host:

- DC01 — Windows Server evaluation, Active Directory domain `lab.local` (in progress)
- LabSwitch — Hyper-V Internal virtual switch
- Later: a Windows 11 VM joined to the domain (optional)

The useful artefact is PowerShell that creates an OU, a group, and users from `users.csv` (run on the DC after AD DS exists).

## Diagram

```mermaid
flowchart TD
  Host["Host: Windows 11 Pro"]
  SW["LabSwitch Internal"]
  DC["DC01 Windows Server lab.local"]
  PC["PC01 Windows 11 domain join later"]
  Host --> SW
  SW --> DC
  SW --> PC

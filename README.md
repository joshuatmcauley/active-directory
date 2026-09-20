# Active Directory lab (lab.local)

Home lab on Hyper-V, modelled as a small school directory (Bellin Grammar School). Not a real school or workplace. Built to practise first-line work: OUs, groups, a PC on the domain, a staff share, and a Group Policy mapping.

Student numbers are two accounts in an OU, not extra VMs.

## Architecture

![lab.local current](architecture/current-architecture.png)

## What is running

- Host: Hyper-V
- Internal virtual switch (isolated lab network, no internet)
- DC01: Windows Server 2022, `192.168.10.10`
- Domain: `lab.local` (AD DS + DNS)
- WIN11-01: Windows 11 Pro, `192.168.10.20`, DNS `192.168.10.10`
- Domain-joined staff PC; test logon: `LAB\amurphy`

## Identity (school OUs)

- Parent OU: `Bellin-Grammar-School`
- `Staff` — Alex Murphy (`amurphy`)
- `IT` — Sam Lee (`slee`)
- `Students` — Harley Kennedy, Jordan Logan
- `Computers` — WIN11-01
- Groups: `Lab-Staff`, `Lab-Students`
- Starter data: `users.csv` / `sample-data/fictional-users.csv`
- Script (first two users, run on the DC): `scripts/New-LabUsers.ps1`

![School OUs](evidence/08-school-ous.png)

![ADUC early LabUsers](evidence/01-aduc-labusers.png)

![C lab folder](evidence/02-lab-folder.png)

![Get-ADUser](evidence/3-get-aduser-enabled.png)

![Server Manager](evidence/4-server-manager-roles.png)

## File share

- Path on DC: `C:\Staff`
- UNC: `\\DC01\Staff`
- Share: `Lab-Staff` Change; `Administrators` Full Control
- NTFS: `Lab-Staff` Modify
- `Lab-Students` is not granted the share (staff drive only)

## Group Policy

- GPO: `Map-Staff-Drive`
- Linked to: `Bellin-Grammar-School\Staff` and `...\IT`
- Not linked to `Students`
- Result: `S:` maps to `\\DC01\Staff` at logon for staff/IT

![whoami amurphy](evidence/05-whoami-amurphy.png)

![Staff drive S](evidence/06-staff-drive-s.png)

![GPO](evidence/07-gpo-labusers.png)

## What went wrong

- `users.csv` was empty, then saved as `users.csv.txt` because Explorer hid the extension
- First passwords failed domain complexity; the script printed Created while accounts were disabled
- `Administrator` was denied on `\\DC01\Staff` until added on the share — not a member of `Lab-Staff`
- Typing a UNC path in Command Prompt is not the same as `dir` or Explorer
- First ping to the DC failed; the second succeeded once the VM was reachable
- GPMC would not delete the old LabUsers GPO link; linking the same GPO to Staff and IT was enough because users had moved OUs

## What this is not

- Not production
- Not Microsoft Entra ID
- Not Hyper-V as a job title
- Not 40 student PCs
- Not written helpdesk runbooks yet

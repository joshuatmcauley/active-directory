# active-directory

# AD lab (Hyper-V)

This is a fictional lab on Hyper-V. Not a real school or workplace. The domain is lab.local. 

## What i was trying to do

Build 



## What is running 

-Hyper-V on my PC
-Windows Server (Desktop Experience)
-VM name: DC01
-Domain: lab.local. DC IP: 192.168.10.10
-Internal Virtual switch

1. Installed Windows Server in Hyper-V and promoted it to a domain controller (AD DS + DNS).
2. Opened Active Directory Users and Computers (ADUC) and confirmed the domain was there.
3. Made a folder `C:\lab` on the DC.
4. Made a CSV of fictional users (AI generated):

5. Wrote a PowerShell script `New-LabUsers.ps1` that:
   - creates an OU called LabUsers
   - creates a group called Lab-Staff
   - creates users `amurphy` and `slee` from the CSV
   - puts them in that group
6. Ran the script (after a few faults, below).
7. Checked in ADUC: LabUsers contains Alex Murphy, Sam Lee, and Lab-Staff.
## What went wrong
I am writing these down because this is how I actually learned it.
CSV was 0 KB
I created the file but it was empty. The script has nothing to import if the file has no rows. Had to paste the three lines and save it.
Explorer lied about the name
File looked like `users.csv`. PowerShell said it could not find `C:\lab\users.csv`. Real name was `users.csv.txt` because Notepad saved it as a text document. `Get-ChildItem C:\lab` showed the truth. Renamed it, then the script could read it.
Password too weak
First run: domain rejected the temp password (complexity). The script still printed "Created amurphy" and "Created slee". That was misleading. `New-ADUser` had failed. The accounts existed but were Disabled.
Skip already exists
Second run skipped both users, so the bad/disabled state stayed. I had to reset the passwords with `Set-ADAccountPassword` and then `Enable-ADAccount`.
Paste went in reverse
I pasted a block of commands and PowerShell ran Enable before the password was set. Enable failed for the same complexity reason. Doing the commands one line at a time fixed it.
After a proper password (length + mixed characters), both users show Enabled True.
## What I can show
- `evidence/` screenshots (no passwords)
- `scripts/New-LabUsers.ps1` (the copy I keep in git, not a live admin password)
- `sample-data/fictional-users.csv`


# Historical ticket requirements

Original fictional hospital scenarios. Historical checklists and status labels are not current completion evidence. Individual case records are in the [Ticket Log](../../tickets/README.md).

## Tickets

<a id="chg0001"></a>

### CHG0001 · Change · P4 Low

```
Requested by: Mark Torres (IT Manager)     Assignment group: IT
Short description: Build the hospital folder structure in Active Directory
Description: Our Active Directory is messy from testing. Please set up the OU
(folder) layout from the IT standards document so we can apply policies to
nursing stations and delegate later.
Rollback plan: Snapshot dc01 before starting.
```

**Done when:**
- [ ] The layout matches **Active Directory folder layout** in [COMPANY.md](COMPANY.md)
- [ ] Test objects (old `Company`, `HR-Staff`, `IT-Helpdesk` folders and similar) are cleaned up or moved into the new layout
- [ ] Ana Cruz is in `Tala → Users → HR`; the Windows 11 PC is in `Tala → Computers → Workstations`
- [ ] dc01 is still in **Domain Controllers**
- [ ] Every new folder is protected from accidental deletion

**Resolution notes:**
```

```

---

<a id="chg0002"></a>

### CHG0002 · Change · P4 Low

```
Requested by: Mark Torres (IT Manager)     Assignment group: IT
Short description: Create department and role groups
Description: Create a security group for every department, plus SG-RDP-Users,
SG-IT-Helpdesk and SG-USB-Allowed. Put a description on each group.
```

**Done when:**
- [ ] `SG-Mgmt-Staff`, `SG-HR-Staff`, `SG-Billing-Staff`, `SG-Medical-Staff`, `SG-Nursing-Staff`, `SG-Lab-Staff`, `SG-Pharmacy-Staff`, `SG-IT-Staff` exist
- [ ] `SG-RDP-Users`, `SG-IT-Helpdesk`, `SG-USB-Allowed` exist
- [ ] All are in `Tala → Groups`, Global scope, Security type, with a description

**Resolution notes:**
```

```

---

<a id="chg0003"></a>

### CHG0003 · Change · P3 Moderate

```
Requested by: Mark Torres (IT Manager)     Assignment group: IT
Short description: Create your IT accounts, stop using Administrator
Description: Per policy, IT staff use a normal account for daily work and
separate accounts for admin work. Create yours. After this, do not log in
with the built-in Administrator.
```

**Done when:**
- [ ] Normal account `firstname.lastname` in `Tala → Users → IT`, member of `SG-IT-Staff`
- [ ] `adm.firstname.lastname` in `Tala → Admin Accounts`, member of **Domain Admins**
- [ ] `hd.firstname.lastname` in `Tala → Admin Accounts`, member of `SG-IT-Helpdesk` only
- [ ] You can log in to dc01 by Remote Desktop with the `adm.` account

**Resolution notes:**
```

```

---

<a id="chg0004"></a>

### CHG0004 · Change · P4 Low

```
Requested by: Mark Torres (IT Manager)     Assignment group: IT
Short description: Rename the Windows 11 PC to the naming standard
Description: The test PC is a general office desktop at the main hospital.
Rename it to the naming standard.
```

**Done when:**
- [ ] The PC is named `MAIN-DT-001`
- [ ] Its Active Directory computer object shows the new name and is in `Workstations`
- [ ] A domain user can still log in afterwards
- [ ] Remote Desktop from the Mac still works

**Resolution notes:**
```

```

---

<a id="chg0005"></a>

### CHG0005 · Change · P3 Moderate

```
Requested by: Mark Torres (IT Manager)     Assignment group: IT
Short description: Snapshots and auto-start for all virtual machines
Description: The hospital runs 24/7. After a power outage, servers must come
back without anyone touching them. Also take a clean baseline snapshot.
```

**Done when:**
- [ ] dc01 and MAIN-DT-001 start automatically when the host `MAIN-HV01` boots
- [ ] dc01 starts **before** the nursing station (the PC needs the domain controller)
- [ ] Each has a checkpoint named `baseline-YYYY-MM-DD`
- [ ] Test: restart `MAIN-HV01`, and both machines come up by themselves

**Resolution notes:**
```

```

---

<a id="ritm0001"></a>

### RITM0001 · Service request · P3 Moderate

```
Requested by: Maria Reyes (HR Manager)     Assignment group: IT Helpdesk
Short description: Onboard all current staff
Description: Hi IT, attached is our staff list. Please create accounts for
everyone so we can stop using the shared "nurse1" login. Thanks! – Maria
Attachment: COMPANY.md → People (staff roster)
```

**Done when:**
- [ ] Every person in the roster has an account (skip "Me" and anyone already created), following the naming rules
- [ ] Each is in the correct department folder and department group
- [ ] Job title, Department, Company, Office and Manager are filled in; doctors' display names start with "Dr."
- [ ] Each has a temporary password with "must change at next logon"
- [ ] Test: log in on MAIN-DT-001 as one new user and change the password

**Resolution notes:**
```

```

---

<a id="ritm0002"></a>

### RITM0002 · Service request · P3 Moderate

```
Requested by: Dr. Dwane Santos (Emergency Physician)     Assignment group: IT
Short description: Remote Desktop to the ER nursing station
Description: I need to check the ER station from the doctors' lounge PC during
night shift. Can I have Remote Desktop access? – Dr. Santos
Approval: Dr. Elena Ramos (Medical Director) approved.
```

**Done when:**
- [ ] Dr. Santos gets access **through the group** `SG-RDP-Users`, not added directly on the PC
- [ ] A Group Policy named by the standard (for example `C - Workstations - Remote Desktop`) puts `SG-RDP-Users` into the PC's **Remote Desktop Users** group
- [ ] The policy is linked to the folder the PC is in (`Workstations`), or to both `Workstations` and `Nursing Stations`
- [ ] Any manual entries you added earlier on the PC (like Ana) are removed, so you know the policy did it
- [ ] Dr. Santos can connect from the Mac with Windows App

**Resolution notes:**
```

```

---

<a id="chg0006"></a>

### CHG0006 · Change · P3 Moderate

```
Requested by: Mark Torres (IT Manager)     Assignment group: IT
Short description: Hospital password and lockout policy
Description: Data privacy audit finding: no password rules and no lockout. Set:
minimum 12 characters, complexity on, maximum age 90 days, remember last
5 passwords. Lock the account after 5 wrong attempts for 15 minutes.
Rollback plan: Snapshot dc01 first.
```

**Done when:**
- [ ] The settings above are in the **Default Domain Policy**
- [ ] Test: setting a 6-character password on a test user is rejected
- [ ] Test: 5 wrong passwords on MAIN-DT-001 lock the user

**Resolution notes:**
```

```

---

<a id="inc0001"></a>

### INC0001 · Incident · P2 High

```
Caller: Dr. Dwane Santos (Emergency Physician)     Channel: Phone, 02:10 AM
Short description: Can't log in at ER station, forgot password
Description: "IT? I just came back from leave and forgot my password. I have
a patient waiting, I need to see the lab results now." – Dr. Santos
Why P2: one person, but patient care is blocked (hospital rule: one level up).
```

**Done when:**
- [ ] Identity verified first (write how in your notes; at 2 AM: confirm with the ER charge nurse on duty, or call back the number in HR records)
- [ ] Password reset, must change at next logon
- [ ] Dr. Santos (you) logs in on MAIN-DT-001 and sets a new password that meets the new policy

**Resolution notes:**
```

```

---

<a id="inc0002"></a>

### INC0002 · Incident · P3 Moderate

```
Caller: Joy Aquino (Staff Nurse, Ward 3)     Channel: Walk-in, 07:05 AM
Short description: "Your account has been locked out"
Description: "It says my account is locked. I only typed my password a few
times! Shift handover is in 10 minutes." – Joy
MAIN-DT-001 until it locks.
```

**Done when:**
- [ ] You found **where** the lockout came from (which computer). Real helpdesks must answer this
- [ ] Account unlocked **without** resetting the password (she knows it)
- [ ] Joy logs in

**Resolution notes:**
```

```

---

<a id="chg0007"></a>

### CHG0007 · Change · P3 Moderate

```
Requested by: Mark Torres (IT Manager)     Assignment group: IT
Short description: Helpdesk may reset passwords, nothing more
Description: The night-shift helpdesk must reset passwords and unlock accounts
for staff, but must not be Domain Admins. Delegate only that.
```

**Done when:**
- [ ] `SG-IT-Helpdesk` has reset-password and unlock rights on `Tala → Users` only
- [ ] The Active Directory admin tools are installed on MAIN-DT-001
- [ ] Logged in as `hd.firstname.lastname`: resetting Ana's password **works**
- [ ] Same account: deleting Ana **fails** with "Access is denied"
- [ ] Same account: resetting your `adm.` account's password **fails**

**Resolution notes:**
```

```

---

<a id="ritm0003"></a>

### RITM0003 · Service request · P4 Low

```
Requested by: Rhea Bautista (Chief Nurse)     Assignment group: IT Helpdesk
Short description: New nurse Monday: Ana Cruz (Nursing)
Description: New Staff Nurse for Ward 3 starting Monday. Her name is
Ana Lopez Cruz. Same name as Ana in HR, sorry! – Rhea
```

**Done when:**
- [ ] New account follows the duplicate-name rule in COMPANY.md
- [ ] Display name lets people tell the two apart (for example `Ana L. Cruz`)
- [ ] Correct folder, group, title, manager
- [ ] HR's Ana is untouched and still logs in

**Resolution notes:**
```

```

---

<a id="ritm0004"></a>

### RITM0004 · Service request · P2 High

```
Requested by: Dr. Elena Ramos (Medical Director)     Assignment group: IT
Short description: Patient Records folder, clinical staff only
Description: We need a shared folder for scanned patient records.
Doctors and nurses: read and edit. Laboratory: read only.
Billing, HR and everyone else: no access at all (Data Privacy Act).
Why P2: privacy compliance for the whole clinical side.
```

**Done when:**
- [ ] A shared folder named `PatientRecords` exists on dc01 (real hospitals use a separate file server; the lab uses dc01)
- [ ] `FS-PatientRecords-Modify` contains `SG-Medical-Staff` and `SG-Nursing-Staff`
- [ ] `FS-PatientRecords-Read` contains `SG-Lab-Staff`
- [ ] Access is given to the FS groups only, never to people directly
- [ ] As a nurse: create and edit a file ✅
- [ ] As Ramon (Laboratory): open ✅, edit ❌
- [ ] As Jessa (Billing): open → access denied ❌

**Resolution notes:**
```

```

---

<a id="ritm0005"></a>

### RITM0005 · Service request · P4 Low

```
Requested by: Rhea Bautista (Chief Nurse)     Assignment group: IT
Short description: Patient Records as drive P:
Description: Nurses keep losing the folder path. Can it just appear as P:?
```

**Done when:**
- [ ] A user policy named by the standard maps `P:` to the PatientRecords share for clinical staff only (Medical, Nursing, Laboratory)
- [ ] A nurse sees `P:` after logging in; Jessa (Billing) does not

**Resolution notes:**
```

```

---

<a id="chg0008"></a>

### CHG0008 · Change · P2 High

```
Requested by: Data Privacy Officer, approved by Mark Torres     Assignment group: IT
Short description: Nursing stations: auto-lock and hide last user
Description: A patient's relative saw records on an unattended nursing station.
Nursing station PCs must lock after 5 minutes without use, and the sign-in
screen must not show who logged in last.
```

**Done when:**
- [ ] A computer policy named by the standard, linked to `Nursing Stations` only
- [ ] Lab test: **move** MAIN-DT-001 into `Nursing Stations`, restart, check the policy applies; move it back to `Workstations`, restart, check it no longer applies (this is how one VM tests both kinds of PC)
- [ ] The PC locks after 5 minutes idle
- [ ] The sign-in screen does not show the last user's name
- [ ] Office PCs in `Workstations` are not affected

**Resolution notes:**
```

```

---

<a id="chg0009"></a>

### CHG0009 · Change · P3 Moderate

```
Requested by: Data Privacy Officer, approved by Mark Torres     Assignment group: IT
Short description: Block USB storage, except IT
Description: Patient data must not leave on flash drives. Block USB storage
for all staff, except members of SG-USB-Allowed (IT).
```

**Done when:**
- [ ] A policy blocks all removable storage for staff
- [ ] Members of `SG-USB-Allowed` are excluded
- [ ] Test: Hyper-V can't pass a USB stick straight into a VM like Proxmox could. Use **Enhanced Session** (Remote Desktop into the VM with local devices/drives redirected) or test on any real Windows PC joined to the domain. As a normal user it's blocked; as your IT account it opens

**Resolution notes:**
```

```

---

<a id="chg0010"></a>

### CHG0010 · Change · P4 Low

```
Requested by: Mark Torres (IT Manager)     Assignment group: IT
Short description: Allow ping on all hospital PCs
Description: Helpdesk needs to ping PCs to troubleshoot. You enabled it by
hand on one PC; do it for all PCs by policy.
```

**Done when:**
- [ ] A computer policy allows inbound ICMPv4 echo (ping) on `Nursing Stations` and `Workstations`
- [ ] Test: disable the manual rule on MAIN-DT-001, restart, ping from the Mac still works

**Resolution notes:**
```

```

---

<a id="chg0011"></a>

### CHG0011 · Change · P4 Low

```
Requested by: Teresa Villanueva (Hospital Administrator)     Assignment group: IT
Short description: Hospital lock screen with privacy notice
Description: All hospital PCs should show our logo and a short notice:
"Authorized personnel only. Patient data is protected under RA 10173."
```

**Done when:**
- [ ] A picture is stored where every PC can read it
- [ ] A policy sets it as the lock screen
- [ ] A sign-in message (title + text) appears before login
- [ ] MAIN-DT-001 shows both after restart

**Resolution notes:**
```

```

---

<a id="ritm0006"></a>

### RITM0006 · Service request · P2 High

```
Requested by: Maria Reyes (HR Manager)     Assignment group: IT
Short description: Offboard Dr. Paolo Lim, effective immediately
Description: Dr. Lim's contract was terminated today. Remove all access now.
Keep his account 30 days for the medical records audit.
Why P2: a former staff member with access to patient data is a privacy risk.
```

**Done when:**
- [ ] Account disabled (not deleted)
- [ ] His group memberships are written in your notes, then removed
- [ ] Password reset to a random one
- [ ] Moved to `Tala → Disabled Users`
- [ ] Description says: `Disabled YYYY-MM-DD per RITM0006, delete after YYYY-MM-DD`
- [ ] Test: logging in as Dr. Lim fails

**Resolution notes:**
```

```

---

<a id="chg0012"></a>

### CHG0012 · Change · P4 Low

```
Requested by: Mark Torres (IT Manager)     Assignment group: IT
Short description: DNS cleanup
Description: 1) Internet lookups should go through our router.
2) IT should reach the Hyper-V host by name, not IP.
3) We need reverse lookups (IP → name) for troubleshooting.
```

**Done when:**
- [ ] dc01's DNS has the forwarder `192.168.1.1`
- [ ] Record `main-hv01.lab.local` → `192.168.1.12` (created automatically if the host joins the domain; otherwise add it by hand)
- [ ] A reverse lookup zone for `192.168.1.x` exists, with records for dc01 and MAIN-DT-001
- [ ] Verify DNS from the Mac by querying dc01 directly

**Resolution notes:**
```

```

---

<a id="chg0013"></a>

### CHG0013 · Change · P3 Moderate

```
Requested by: Mark Torres (IT Manager)     Assignment group: IT
Short description: Weekly backups of all virtual machines
Description: We have snapshots but no backups. A snapshot is not a backup.
Set up weekly backups and prove a restore works.
```

**Done when:**
- [ ] A weekly backup of dc01 and MAIN-DT-001 runs from the host, keeping the last 2 (Windows Server Backup on `MAIN-HV01`, or **Veeam Community Edition**, free)
- [ ] One backup has run successfully
- [ ] **Restore test:** restore the nursing station backup as a new virtual machine ID, boot it, then delete it
- [ ] Notes explain why a snapshot is not a backup

**Resolution notes:**
```

```

---

<a id="chg0014"></a>

### CHG0014 · Change · P3 Moderate

```
Requested by: Mark Torres (IT Manager)     Assignment group: IT
Short description: Monthly patching in a maintenance window
Description: Microsoft releases updates on the second Tuesday of every month
("Patch Tuesday"). Patch both machines. Hospital rule: patch clinical PCs
only in the maintenance window (Sunday 1–4 AM) and tell the Chief Nurse first.
```

**Done when:**
- [ ] A short notice to the Chief Nurse is written in your notes (what, when, expected downtime)
- [ ] Snapshot before patching
- [ ] Nursing station patched **first**, checked, **then** dc01 (test on less important machines first)
- [ ] After patching: domain logins, Remote Desktop and drive P: still work
- [ ] Old snapshots removed after a few days
- [ ] Notes list what was installed and how long it took

**Resolution notes:**
```

```

---

<a id="chg0015"></a>

### CHG0015 · Change · P3 Moderate · **Resolved 2026-10-17**

```
Requested by: Mark Torres (IT Manager)     Assignment group: IT
Short description: Build a Hyper-V host for the hospital's systems
Description: Standardize on Microsoft Hyper-V. Replace the old Proxmox host with
Windows Server + Hyper-V and run the domain controller on it.
Rollback plan: none (rebuild); dc01 was rebuilt from scratch.
```

**Done when:**
- [x] Laptop reinstalled as `MAIN-HV01`: Windows Server 2025 Standard (Desktop Experience), static IP `192.168.1.12`
- [x] Hyper-V role installed
- [x] An **external virtual switch** connects VMs to the network
- [x] A **Generation 2** VM (dc01) runs on it with network access
- [x] `192.168.1.12` added to the IP plan in COMPANY.md

**Resolution notes:**
```
Changed: Built Windows install USB on the Mac (FAT32, install.wim split with wimlib),
wiped Proxmox, installed Windows Server 2025 Standard on the laptop. Realtek RTL8168
worked with the built-in driver (Realtek Server 2025 driver kept on the USB as backup).
Lid action = do nothing, Remote Desktop on, Hyper-V role + external switch.
Issue: after creating the switch, the static IP stayed on the physical adapter and the
new vEthernet adapter took a DHCP address; old .10 also lingered. Cleared the physical
adapter (IPv4 back to automatic, then unbound), set 192.168.1.12 + gateway on vEthernet,
removed extra addresses. Ping blocked: built-in ICMP rules not usable on the current
profile, so created a custom inbound rule (ICMPv4 Echo Request, all profiles).
Created dc01 (Gen 2, 4 GB, 60 GB dynamic VHDX), installed Windows Server 2025 Standard,
promoted to lab.local.
Verified: ipconfig /all shows only 192.168.1.12 on vEthernet with gateway; ping and
Remote Desktop to .12 and .10 from the Mac; dc01 DNS answers lab.local and the DC record.
Advice: always set the host's static IP AFTER creating the external switch.
```

---

<a id="chg0016"></a>

### CHG0016 · Change · P4 Low

```
Requested by: Mark Torres (IT Manager)     Assignment group: IT
Short description: Hyper-V operations
Description: Before the clinic goes live, prove you can run Hyper-V day to day:
checkpoints before changes, moving a VM, and sharing memory well.
```

**Done when:**
- [ ] Take a **checkpoint** of a test VM (not dc01), change something, **apply** the checkpoint, and the change is gone
- [ ] Explain in notes: Standard vs Production checkpoints
- [ ] **Export** the test VM, delete it, **import** it back, and it boots
- [ ] **Dynamic Memory** turned on for the test VM; notes say what minimum/startup/maximum mean
- [ ] Notes include the Proxmox ↔ Hyper-V name table from memory (bridge = ?, snapshot = ?, and so on)

**Resolution notes:**
```

```

---

<a id="chg0017"></a>

### CHG0017 · Change · P3 Moderate

```
Requested by: Rhea Bautista (Chief Nurse)     Assignment group: IT
Short description: Print server for Ward 3, deployed by policy
Description: Ward 3 got a network printer. Nurses should get it automatically
on any nursing station, without IT installing it on each PC.
Lab note: there is no real printer. Create a printer on the server with a
standard TCP/IP port pointing to an unused address (for example 192.168.1.30)
and a generic driver. The jobs will just wait in the queue, which is enough.
```

**Done when:**
- [ ] Print server role installed on dc01 (real hospitals use a separate server)
- [ ] Shared printer `MAIN-W3-PRN01` on port `192.168.1.30`, address added to the IP plan as a printer
- [ ] A policy named by the standard deploys it to nursing staff (user policy) or nursing stations (computer policy). Write in your notes which one you chose and why
- [ ] A nurse logs in on MAIN-DT-001 and sees the printer without installing anything
- [ ] A test print appears in the server's queue

**Resolution notes:**
```

```

---

<a id="inc0003"></a>

### INC0003 · Incident · P2 High

```
Caller: Joy Aquino (Staff Nurse, Ward 3)     Channel: Phone, 10:40 AM
Short description: Can't print discharge papers
Description: "Nothing prints! Three patients are waiting to go home and I
need their discharge papers." – Joy
Why P2: one department, patient flow blocked.
```

**Done when:**
- [ ] Cause found and written in work notes (with the steps that narrowed it down)
- [ ] Printing works again for Ward 3
- [ ] Notes say how to prevent it or spot it faster next time

**Resolution notes:**
```

```

---


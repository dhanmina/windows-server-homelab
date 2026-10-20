# Tala General Hospital — Fictional organization standards

A **fictional** hospital used for the [simulated ticket scenarios](TICKETS.md). Staff identities and organizational requirements below are fictional scenario data, not real hospital records.

**Why a hospital:** besides the normal departments (HR, Finance, IT), hospitals add things employers value:
- **Shared PCs** at nursing stations, where many people log in to the same computer every shift
- **24/7 operation**, so priorities matter (a problem in the Emergency Room beats a problem in Billing)
- **Patient privacy:** the Data Privacy Act of 2012 (Republic Act 10173) requires that only the right staff can see patient data. Permissions, auto-lock and USB blocking have a real reason here
- **Doctors and nurses move between PCs**, so accounts must work everywhere

---

## Hospital profile

| | |
|---|---|
| Name | Tala General Hospital (fictional) |
| Type | Private general hospital, ~100 beds |
| Main hospital | site code **MAIN** |
| Outpatient clinic (small branch) | site code **CLN** |
| Domain | `lab.local` (short name `LAB`) |
| My role | IT Support Specialist (I also do sysadmin work, because IT is small) |
| My boss | Mark Torres, IT Manager (approves changes) |
| Operating hours | 24/7. Office departments 8 AM–5 PM |

**Note about the domain name:** real organizations today use a subdomain of a domain they own (for example `corp.talageneral.ph`), not `.local`. `lab.local` is fine for practice.

---

## People (staff roster)

Fictional staff roster for the onboarding scenario.

| Name | Job title | Department | Site | Manager |
|---|---|---|---|---|
| Elena Ramos | Medical Director | Management | MAIN | — |
| Teresa Villanueva | Hospital Administrator | Management | MAIN | Elena Ramos |
| Maria Reyes | HR Manager | HR | MAIN | Teresa Villanueva |
| Ana Cruz | HR Officer | HR | MAIN | Maria Reyes |
| Liza Mendoza | Billing Supervisor | Billing | MAIN | Teresa Villanueva |
| Jessa Garcia | Billing Clerk | Billing | MAIN | Liza Mendoza |
| Dwane Santos | Emergency Physician | Medical | MAIN | Elena Ramos |
| Paolo Lim | Pediatrician | Medical | CLN | Elena Ramos |
| Rhea Bautista | Chief Nurse | Nursing | MAIN | Elena Ramos |
| Carlo Dizon | Staff Nurse, Emergency Room | Nursing | MAIN | Rhea Bautista |
| Joy Aquino | Staff Nurse, Ward 3 | Nursing | MAIN | Rhea Bautista |
| Ramon Tan | Medical Technologist | Laboratory | MAIN | Elena Ramos |
| Bea Flores | Pharmacist | Pharmacy | MAIN | Teresa Villanueva |
| Mark Torres | IT Manager | IT | MAIN | Teresa Villanueva |
| Lab operator | IT Support Specialist | IT | MAIN | Mark Torres |

Doctors' display names start with "Dr." (for example `Dr. Dwane Santos`); logon names do not.

---

## Naming conventions (the hospital's IT rules)

Real IT departments write these rules down so every IT person creates things the same way. Follow them in every ticket.

### User accounts

| What | Rule | Example |
|---|---|---|
| Logon name | `firstname.lastname`, lowercase | `ana.cruz` |
| Same name already exists | add middle initial: `firstname.m.lastname` | `ana.l.cruz` |
| No middle name either | add a number | `ana.cruz2` |
| Display name | `Firstname Lastname`, doctors `Dr. Firstname Lastname` | `Dr. Dwane Santos` |
| Fill in on every user | Job title, Department, Company, Office (site code), Manager | — |
| First password | Temporary, plus **"User must change password at next logon"** | — |

### IT staff accounts (one person, separate accounts by power)

| Account | Rule | Example | Used for |
|---|---|---|---|
| Normal | `firstname.lastname` | `juan.delacruz` | Email, daily work |
| Helpdesk | `hd.firstname.lastname` | `hd.juan.delacruz` | Password resets, unlocks (limited power) |
| Admin | `adm.firstname.lastname` | `adm.juan.delacruz` | Server and Active Directory changes (full power) |
| Service account | `svc-purpose` | `svc-backup` | Programs that need to log in, not people |

The built-in `Administrator` is for emergencies only.

### Computers

Format: `SITE-TYPE-NUMBER`, maximum 15 characters (a Windows limit).

| Type | Code | Example |
|---|---|---|
| Nursing station (shared PC) | `NS` | `MAIN-NS-001` |
| Office desktop | `DT` | `MAIN-DT-001` |
| Laptop | `LT` | `CLN-LT-001` |
| Server | role + number | `MAIN-DC01` (domain controller), `MAIN-FS01` (file server) |

**Exception:** my domain controller stays `dc01`. Renaming a domain controller is risky, so it is not worth it in the lab.

### Groups

| Kind | Rule | Example | Used for |
|---|---|---|---|
| Department | `SG-Dept-Staff` | `SG-Nursing-Staff` | "Everyone in Nursing" |
| Role or access | `SG-Purpose` | `SG-RDP-Users`, `SG-IT-Helpdesk`, `SG-USB-Allowed` | Special permissions |
| Shared folder access | `FS-Folder-Level` | `FS-PatientRecords-Modify`, `FS-PatientRecords-Read` | Who can open or change a shared folder |

`SG` = Security Group. `FS` = File Share. Description field on every group: what it is for and who approved it.

### Group Policy (settings pushed from the domain controller)

Format: `C` or `U` - where - purpose.

| Start | Meaning | Example |
|---|---|---|
| `C -` | Computer settings (apply to the PC, whoever logs in) | `C - Nursing Stations - Auto Lock` |
| `U -` | User settings (follow the person to any PC) | `U - Clinical - Map P Drive` |

---

## Active Directory folder layout (OUs)

OU = Organizational Unit, a folder inside Active Directory.

```
lab.local
├─ Domain Controllers      (built-in, dc01 stays here, never move it)
└─ Tala
   ├─ Users
   │  ├─ Management
   │  ├─ HR
   │  ├─ Billing
   │  ├─ Medical
   │  ├─ Nursing
   │  ├─ Laboratory
   │  ├─ Pharmacy
   │  └─ IT
   ├─ Computers
   │  ├─ Nursing Stations  (shared PCs)
   │  ├─ Workstations      (personal office PCs and laptops)
   │  └─ Servers
   ├─ Groups
   ├─ Admin Accounts       (hd. and adm. accounts)
   ├─ Service Accounts
   └─ Disabled Users       (people who left, kept 30 days)
```

---

## IP address plan (home network 192.168.1.0/24)

| Address | Device | How it gets the address |
|---|---|---|
| `192.168.1.1` | Router (OpenWrt) | Static |
| `192.168.1.10` | dc01, domain controller + DNS | Static |
| `192.168.1.11` | reserved for a second domain controller | — |
| `192.168.1.12` | **MAIN-HV01**, the laptop: Windows Server 2025 Standard + Hyper-V host | Static (on the vEthernet adapter) |
| `192.168.1.20` | reserved for a file server | — |
| `192.168.1.30`–`.39` | reserved for printers | Static or reservation |
| `192.168.1.100`–`.249` | PCs and phones | DHCP (automatic, from the router) |

Domain PCs always use **`192.168.1.10`** as DNS.

**My lab has one Windows 11 virtual machine: `MAIN-DT-001`** (general office desktop, in `Workstations`). Every staff member logs in on it, one at a time. To test nursing-station policies, move its computer object into `Nursing Stations`, restart, test, then move it back. The laptop's CPU (Intel i5-7200U, 2 cores / 4 threads) and 14 GB RAM fit about 3 VMs at once, so one test PC is enough.

# Lab environment and evidence

## Purpose

Practice Windows infrastructure administration in a fictional hospital scenario. Tala General Hospital is a training organization; this repository does not represent a production hospital deployment.

## Documented configuration

- Host: `MAIN-HV01`, Windows Server 2025 with Hyper-V.
- Domain controller / DNS lab role: `MAIN-DC-001`, domain `lab.local`.
- Additional VMs: `LAB-WIN11-01` and `MAIN-DT-001`.
- Department OUs: Administration, Doctors, IT, Laboratory, Nurses, Pharmacy.
- GPO: `GPO_Nurses_RestrictSettings`, linked to the Nurses OU.

Host OS and server roles come from the supplied lab description. The original screenshots are preserved without editing.

## Evidence index

| Screenshot | Directly visible | Not established |
| --- | --- | --- |
| [Hyper-V inventory](../screenshots/virtual-machines.png) | Host name, three VM names, displayed states, assigned memory for running VMs, configuration versions | Guest OS editions, domain membership, service health |
| [AD OUs](../screenshots/organizational-units.png) | Domain and department OU names | OU contents, access delegation, account configuration |
| [GPO scope](../screenshots/group-policy.png) | GPO name, Nurses link, link enabled, not enforced, Authenticated Users filter, no WMI filter | Individual settings, client application, effective restrictions |

Older planning notes may use other names or OU layouts. This deliverable uses the supplied current names and the OU structure visible in the screenshots. No network addresses or additional deployed services are inferred.

## Ticket Log

Simulated IT Support and infrastructure change exercises are documented in the [Ticket Log](../tickets/README.md). Each record distinguishes documented work from outcomes that could not be independently verified.

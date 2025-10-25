---
layout: post
title: "setting up r630"
categories:
  - idrac
  - poweredge
  - bios
  - bmc
---

setting up r630

- 56 cores
- 512 GB RAM
- few ssds and an nvme
- A1000 GPU
- 10 GbE

nvme drive not visible in ubuntu, investigation points to firmware

down the rabbithole to update

P.S. Updating iDRAC to latest did not change anything. BIOS updates got to 2.13.0 and failed after. 

Neither fixed the nvme drive not showing up in ubuntu

### updating firmware

starting with 2.50.50.50 newest is 2.86.86.86

- first attempt 2.50.50.50 -> 2.86.86.86 failed
- smallest possible jump 2.50.50.50 -> 2.52.52.52 also failed
- trying to update BIOS first

#### updating BIOS (~ 2 hrs)

first quirk is needing to use Windows x64 `.exe` packages to update the BIOS via the iDRAC GUI. wack. Is iDRAC running
Windows?

starting with 2.8.0 newest is 2.19.0

- 2.8.0 -> 2.19.0 failed with error `RED007: Unable to verify Update Package signature.`
- minimal jump 2.8.0 -> 2.9.1 succeeded
- sequence of updates in table below

| version | update                                 | success                                                                                        |
|---------|----------------------------------------|------------------------------------------------------------------------------------------------|
| 2.8.0   | 2.19.0                                 | ❌                                                                                              |
| 2.8.0   | 2.9.1                                  | ✅                                                                                              |
| 2.9.1   | 2.10.5                                 | ✅                                                                                              |
| 2.10.5  | 2.11.0, 2.12.1, 2.13.0                 | ✅ <br/>  turns out multiple updates can be queued at once and applied in a single reboot cycle | 
| 2.13.0  | 2.15.0, 2.16.0, 2.17.0, 2.18.1, 2.19.0 | ❌ <br/> `RED007: Unable to verify Update Package signature.`                                   | 
| 2.13.0  | 2.15.0, 2.16.0                         | ❌ <br/> `RED007: Unable to verify Update Package signature.`                                   | 
| 2.13.0  | 2.15.0                                 | ❌ <br/> `RED007: Unable to verify Update Package signature.`                                   | 

2.13.0 is as far as we go I guess

#### updating iDRAC (~ 4 hrs)

again, the Windows x64 packages are used. wack

| version    | update                 | success                                                     |
|------------|------------------------|-------------------------------------------------------------|
| 2.50.50.50 | 2.86.86.86             | ❌<br/> `RED007: Unable to verify Update Package signature.` |
| 2.50.50.50 | 2.52.52.52, 2.60.60.60 | ✅                                                           |
| 2.60.60.60 | 2.61.60.60, 2.63.60.61 | ✅                                                           |
| 2.63.60.61 | 2.70.70.70, 2.75.75.75 | ✅                                                           |
| 2.75.75.75 | 2.80.80.80, 2.81.81.81 | ✅                                                           |



### bare metal os
running [netbootxyz](https://netboot.xyz/) on [truenas](https://www.truenas.com/truenas-community-edition/) and booting over PXE makes trying out OSs easy.
tl;dr;
- requires DHCP server with PXE compatibility
- set boot image in DHCP config to be `netboot.xyz.kpxe`
- boot from PXE compatible NIC
- netboot menu should show up 

#### ubuntu 24.04 LTS
failed installation on several attempts because of some bug in the disk handling. had to remove PVs and VGs manually to work around. 

results were underwhelming compared to previous experience with [proxmox ve](https://www.truenas.com/truenas-community-edition/)

#### talos
very fast to boot up but clearly not the right choice for a bare metal hypervisor role

#### proxmox
familiar choice and does the job well. main issue with last cluster was sub-par storage. cluster is simply not big enough to make ceph work. pointing at iSCSI from truenas for VM storage this time.

**note**: proxmox's graphical installer just hung. had to use text installer and it was smooth sailing.

now to do this all over again on another r630

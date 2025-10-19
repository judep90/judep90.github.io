---
layout: post
title: "[wip] setting up r630"
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

### updating iDRAC

starting with 2.50.50.50. newest is 2.86.86.86.

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

---
layout: post
title:  "[wip] setting up r630"
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
- first attempt  2.50.50.50 -> 2.86.86.86 failed 
- smallest possible jump 2.50.50.50 -> 2.52.52.52 also failed
- trying to update BIOS first

#### updating BIOS
starting with 2.8.0 newest is 2.19.0
- 2.8.0 -> 2.19.0 failed with error `RED007: Unable to verify Update Package signature.`
- minimal jump 2.8.0 -> 2.9.1 succeeded

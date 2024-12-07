---
layout: post
title:  "[WIP] - qgroundcontrol on apple silicon"
categories: qgc qgroundcontrol macos arm
---

Installing [QGroundControl](https://docs.qgroundcontrol.com/master/en/qgc-user-guide/index.html) through `brew` on macos pulls down an `x86` build. Should be able to compile qgc for `arm64`.
Biggest PITA is getting [Qt5](https://doc.qt.io/qt-5/) compiled from source necessitated by the optional features required by QGC that are not part of a standard Qt SDK installation.

### Build box
- [Mac Mini M1 2020](https://support.apple.com/en-us/111894)
- MacOS 15.1.1
- XCode 16.1
- Homebrew 4.4.10

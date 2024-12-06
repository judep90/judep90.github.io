---
layout: post
title:  "[WIP] - qgroundcontrol on apple silicon"
categories: qgc qgroundcontrol macos arm
---

Installing [QGroundControl](https://docs.qgroundcontrol.com/master/en/qgc-user-guide/index.html) through `brew` on macos pulls down an `x86` build. Should be able to compile qgc for `arm64`.
Biggest PITA is getting [Qt5](https://doc.qt.io/qt-5/) compiled from source. 

### Build box
- [Mac Mini M1 2020](https://support.apple.com/en-us/111894)
- MacOS 15.1.1
- Homebrew 4.4.10

### Compile Qt5 from Source
This will be painful.
Official instructions [here](https://wiki.qt.io/Building_Qt_5_from_Git#macOS).

#### Condensed steps
- Get source 
    ```
    git clone git clone git://code.qt.io/qt/qt5.git
    cd qt5
    git checkout v5.15.0
    perl init-repository
    ```
  get a ~~snack~~ meal



---
layout: post
title:  "qgroundcontrol on apple silicon"
categories: qgc qgroundcontrol macos arm qt
---

Installing [QGroundControl](https://docs.qgroundcontrol.com/master/en/qgc-user-guide/index.html) through `brew` on macos pulls down an `x86` build. Should be able to compile qgc for `arm64`.

### Build box
- [Mac Mini M1 2020](https://support.apple.com/en-us/111894)
- MacOS 15.1.1
- XCode 16.1
- Qt 5.15 & 6.7.3 - `brew install qt qt@5`
  - `qmlglsink` does not build without `6.7.3`
- GStreamer - `brew install gstreamer`

### Build
```
git clone --recursive -j8 https://github.com/mavlink/qgroundcontrol.git
cd qgroundcontrol
git checkout Stable_V4.4
git submodule update --init --recursive
cmake . -G "Unix Makefiles" 
make --jobs=6 QGroundControl
```

### Run 
```
./QGroundControl.app/Contents/MacOS/QGroundControl
```
![Ta Da!](/assets/posts/2024-12-06-qgroundcontrol-on-apple-silicon/img.png)
Not sure why Activity Monitor does not have a name but the highlighted row is QGC running without Rosetta.
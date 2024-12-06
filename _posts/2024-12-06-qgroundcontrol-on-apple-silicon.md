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
- Configure
  ```
  ./configure -opensource -confirm-license -nomake examples -nomake tests
  Qt modules and options:
    Qt Concurrent .......................... yes
    Qt D-Bus ............................... yes
    Qt D-Bus directly linked to libdbus .... no
    Qt Gui ................................. yes
    Qt Network ............................. yes
    Qt Sql ................................. yes
    Qt Testlib ............................. yes
    Qt Widgets ............................. yes
    Qt Xml ................................. yes
  Support enabled for:
    Using pkg-config ....................... no
    udev ................................... no
    Using system zlib ...................... yes
    Zstandard support ...................... no
  Qt Core:
    DoubleConversion ....................... yes
      Using system DoubleConversion ........ no
    GLib ................................... no
    iconv .................................. no
    ICU .................................... no
    Built-in copy of the MIME database ..... yes
    Tracing backend ........................ <none>
    Logging backends:
      journald ............................. no
      syslog ............................... no
      slog2 ................................ no
    PCRE2 .................................. yes
      Using system PCRE2 ................... no
  Qt Network:
    CoreWLan ............................... yes
    getifaddrs() ........................... yes
    IPv6 ifname ............................ yes
    libproxy ............................... no
    SecureTransport ........................ yes
    OpenSSL ................................ no
      Qt directly linked to OpenSSL ........ no
    OpenSSL 1.1 ............................ no
    DTLS ................................... no
    OCSP-stapling .......................... no
    SCTP ................................... no
    Use system proxies ..................... yes
    GSSAPI ................................. yes
  Qt Gui:
    Accessibility .......................... yes
    FreeType ............................... yes
      Using system FreeType ................ no
    HarfBuzz ............................... yes
      Using system HarfBuzz ................ no
    Fontconfig ............................. no
    Image formats:
      GIF .................................. yes
      ICO .................................. yes
      JPEG ................................. yes
        Using system libjpeg ............... no
      PNG .................................. yes
        Using system libpng ................ no
    Text formats:
      HtmlParser ........................... yes
      CssParser ............................ yes
      OdfWriter ............................ yes
      MarkdownReader ....................... yes
        Using system libmd4c ............... no
      MarkdownWriter ....................... yes
    EGL .................................... no
    OpenVG ................................. no
    OpenGL:
      Desktop OpenGL ....................... yes
      OpenGL ES 2.0 ........................ no
      OpenGL ES 3.0 ........................ no
      OpenGL ES 3.1 ........................ no
      OpenGL ES 3.2 ........................ no
    Vulkan ................................. no
    Session Management ..................... yes
  Features used by QPA backends:
    evdev .................................. no
    libinput ............................... no
    INTEGRITY HID .......................... no
    mtdev .................................. no
    tslib .................................. no
    xkbcommon .............................. no
    X11 specific:
      XLib ................................. no
      XCB Xlib ............................. no
      EGL on X11 ........................... no
      xkbcommon-x11 ........................ no
  QPA backends:
    DirectFB ............................... no
    EGLFS .................................. no
    LinuxFB ................................ no
    VNC .................................... no
  Qt Sql:
    SQL item models ........................ yes
  Qt Widgets:
    GTK+ ................................... no
    Styles ................................. Fusion macOS Windows
  Qt PrintSupport:
    CUPS ................................... yes
  Qt Sql Drivers:
    DB2 (IBM) .............................. no
    InterBase .............................. no
    MySql .................................. no
    OCI (Oracle) ........................... no
    ODBC ................................... no
    PostgreSQL ............................. no
    SQLite2 ................................ no
    SQLite ................................. yes
      Using system provided SQLite ......... no
    TDS (Sybase) ........................... no
  Qt Testlib:
    Tester for item models ................. yes
  Serial Port:
    ntddmodm ............................... no
  Qt SerialBus:
    Socket CAN ............................. no
    Socket CAN FD .......................... no
    SerialPort Support ..................... yes
  Further Image Formats:
    JasPer ................................. no
    MNG .................................... no
    TIFF ................................... yes
      Using system libtiff ................. no
    WEBP ................................... yes
      Using system libwebp ................. no
  Qt QML:
    QML network support .................... yes
    QML debugging and profiling support .... yes
    QML just-in-time compiler .............. yes
    QML sequence object .................... yes
    QML XML http request ................... yes
    QML Locale ............................. yes
  Qt QML Models:
    QML list model ......................... yes
    QML delegate model ..................... yes
  Qt Quick:
    Direct3D 12 ............................ no
    AnimatedImage item ..................... yes
    Canvas item ............................ yes
    Support for Qt Quick Designer .......... yes
    Flipable item .......................... yes
    GridView item .......................... yes
    ListView item .......................... yes
    TableView item ......................... yes
    Path support ........................... yes
    PathView item .......................... yes
    Positioner items ....................... yes
    Repeater item .......................... yes
    ShaderEffect item ...................... yes
    Sprite item ............................ yes
  QtQuick3D:
    Assimp ................................. yes
    System Assimp .......................... no
  Qt Scxml:
    ECMAScript data model for QtScxml ...... yes
  Qt Gamepad:
    SDL2 ................................... no
  Qt 3D:
    Assimp ................................. yes
    System Assimp .......................... no
    Output Qt3D GL traces .................. no
    Use SSE2 instructions .................. yes
    Use AVX2 instructions .................. no
    Aspects:
      Render aspect ........................ yes
      Input aspect ......................... yes
      Logic aspect ......................... yes
      Animation aspect ..................... yes
      Extras aspect ........................ yes
  Qt 3D Renderers:
    OpenGL Renderer ........................ yes
    RHI Renderer ........................... no
  Qt 3D GeometryLoaders:
    Autodesk FBX ........................... no
  Qt Wayland Client ........................ no
  Qt Wayland Compositor .................... no
  Qt Bluetooth:
    BlueZ .................................. no
    BlueZ Low Energy ....................... no
    Linux Crypto API ....................... no
    Native Win32 Bluetooth ................. no
    WinRT Bluetooth API (desktop & UWP) .... no
    WinRT advanced bluetooth low energy API (desktop & UWP) . no
  Qt Sensors:
    sensorfw ............................... no
  Qt Quick Controls 2:
    Styles ................................. Default Fusion Imagine Material Universal
  Qt Quick Templates 2:
    Hover support .......................... yes
    Multi-touch support .................... yes
  Qt Positioning:
    Gypsy GPS Daemon ....................... no
    WinRT Geolocation API .................. no
  Qt Location:
    Qt.labs.location experimental QML plugin . yes
    Geoservice plugins:
      OpenStreetMap ........................ yes
      HERE ................................. yes
      Esri ................................. yes
      Mapbox ............................... yes
      MapboxGL ............................. yes
      Itemsoverlay ......................... yes
  QtXmlPatterns:
    XML schema support ..................... yes
  Qt Multimedia:
    ALSA ................................... no
    GStreamer 1.0 .......................... no
    GStreamer 0.10 ......................... no
    Video for Linux ........................ no
    OpenAL ................................. yes
    PulseAudio ............................. no
    Resource Policy (libresourceqt5) ....... no
    AVFoundation ........................... yes
    Windows Audio Services ................. no
    DirectShow ............................. no
    Windows Media Foundation ............... no
  Qt TextToSpeech:
    Flite .................................. no
    Flite with ALSA ........................ no
    Speech Dispatcher ...................... no
  Qt Tools:
    Qt Assistant ........................... yes
    Qt Designer ............................ yes
    Qt Distance Field Generator ............ yes
    kmap2qmap .............................. yes
    Qt Linguist ............................ yes
    Mac Deployment Tool .................... yes
    makeqpf ................................ yes
    pixeltool .............................. yes
    qdbus .................................. yes
    qev .................................... yes
    Qt Attributions Scanner ................ yes
    qtdiag ................................. yes
    qtpaths ................................ yes
    qtplugininfo ........................... yes
    Windows deployment tool ................ no
    WinRT Runner Tool ...................... no
  Qt Tools:
    QDoc ................................... yes
  Qt WebEngine Build Tools:
    Use System Ninja ....................... yes
    Use System Gn .......................... no
    Jumbo Build Merge Limit ................ 8
    Developer build ........................ no
    Optional system libraries used:
      re2 .................................. no
      icu .................................. no
      libwebp, libwebpmux and libwebpdemux . no
      opus ................................. no
      ffmpeg ............................... no
      libvpx ............................... no
      snappy ............................... no
      glib ................................. no
      zlib ................................. no
      minizip .............................. no
      libevent ............................. no
      jsoncpp .............................. no
      protobuf ............................. no
      libxml2 and libxslt .................. no
      lcms2 ................................ no
      png .................................. no
      JPEG ................................. no
      harfbuzz ............................. no
      freetype ............................. no
      xkbcommon ............................ no
    
  Note: No wayland-egl support detected. Cross-toolkit compatibility disabled.
    
  Note: The following modules are not being compiled in this configuration:
      webenginecore
      webengine
      webenginewidgets
      pdf
      pdfwidgets
  ```
- Build
  ```
  make -j8
  ```
  get a bigger meal and a drink for the inevitable build failure
    

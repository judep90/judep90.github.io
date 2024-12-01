---
layout: post
title:  "mavlink debugging"
categories: mavlink
---
## socat setup
MAVLink over serial can't be introspected (AFAIK) but can be over TCP.
Setup a `socat` bridge for path `device[serial]->socat[tcp]->socat[serial]`
```shell
# device[serial]->socat[tcp]
socat -d2 tcp-l:5760,nodelay /dev/cu.usbmodem01,rawer
```

```shell
# socat[tcp]->socat[serial]
> socat -d2 pty,rawer tcp:localhost:5760,keepalive,nodelay

2024/11/30 11:59:17 socat[3461] N PTY is /dev/ttys007
2024/11/30 11:59:17 socat[3461] N opening connection to LEN=16 AF=2 127.0.0.1:5760
2024/11/30 11:59:17 socat[3461] N successfully connected from local address LEN=16 AF=2 127.0.0.1:51708
2024/11/30 11:59:17 socat[3461] N starting data transfer loop with FDs [5,5] and [9,9]
2024/11/30 11:59:17 socat[3461] N write(5, 0x140010000, 779) completed
```

```shell
# drain serial buffer
> tail -f /dev/ttys007 > /dev/null
```

## wireshark setup
[wireshark lua script](/assets/lua/mavlink_2_common.lua) generated from MAVLink dialect `common`
![wireshark capturing and decoding MAVLink](/assets/posts/2024-11-30-mavlink-wireshark/wireshark-screenshot.png)

<link rel="stylesheet" href="/assets/css/highlightjs/idea.css">
<script src="/assets/js/highlight.min.js"></script>
<script src="/assets/js/json.min.js"></script>
## results
[packet capture](/assets/posts/2024-11-30-mavlink-wireshark/mavlink.pcapng)
<details><summary>Dissected MAVLink Packets</summary> <pre><code class="language-json"> {% include_relative mavlink-packet-dissection.json %} </code></pre> </details> <script>hljs.highlightAll()</script>




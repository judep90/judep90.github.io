---
layout: post
title:  "cheap* data links"
categories: mavlink datalink crossfire tbs
---

goal: mavlink to FC running PX4 over a wireless link

\*sub $100 tx+rx cost

## existing options

| link          | Manufacturer | frequency  | tx cost | rx cost | notes                                                                                           |
|---------------|--------------|------------|---------|---------|-------------------------------------------------------------------------------------------------|
| SiK           | Holybro      | 915MHz     | ~$60    | $0      | <ul><li>kit with pair of tx and rx in plastic enclosure</li><li>100mW & 500mW options</li></ul> |
| SiK           | RFDesign     | 902-928MHz | ~$170   | ~$105   | <ul><li>AES in hardware</li><li>heatsink looks cool</li><ul>                                    |
| Microhard     | Holybro      | 902-928MHz | $225    | $225    |                                                                                                 |
| Microhard     | ARK          | 900MHz     | $500    | $500    | frequently out of stock                                                                         |
| Microhard     | ARK          | 2.4GHz     | $500    | $500    | frequently out of stock                                                                         |
| TBS Crossfire | TBS          | 900MHz     | ~$70    | ~$30    | easy local pickup at ProgressiveRC                                                              |
| TBS Tracer    | TBS          | 2.4GHz     | ~$70    | ~$30    | easy local pickup at ProgressiveRC                                                              |

Not exhaustive and does not attempt to compare qualitatively (power, range, etc.)

## mavlink on TBS Crossfire
- Cheap and easily available [rx](https://www.progressiverc.com/products/tbs-crossfire-nano-receiver-pro) and [tx](https://www.progressiverc.com/products/tbs-crossfire-nano-tx) units.
- Frustratingly opaque configuration software

### current build incorporating TBS Crossfire
Components 
- Holybro Kakute H7 mini v1.3 
- Tekko32 AM32 ESC
- SAM-M10Q GNSS + Compass -> UART4
- RadioMaster RP2 - UART6 
- TBS CRSF Nano Rx Pro -> UART2

![](/assets/posts/2024-12-27-cheap-datalinks/top.jpeg)
![](/assets/posts/2024-12-27-cheap-datalinks/fc.jpeg)

PX4 1.15 built from source with `boardconfig` serial ports mapped
- TELEM1 -> /dev/ttyS1
- GPS1 -> /dev/ttyS3
- RC -> /dev/ttyS4
![](/assets/posts/2024-12-27-cheap-datalinks/boardconfig.png)

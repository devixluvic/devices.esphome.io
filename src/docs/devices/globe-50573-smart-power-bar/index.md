---
title: "Globe 50573 Smart Power Bar"
date-published: 2026-11-20
type: plug
standard: us
board: bk72xx
project-url: https://www.canadiantire.ca/en/pdp/globe-4-outlet-and-2-usb-port-smart-wifi-power-bar-with-surge-protector-compatible-with-alexa-and-google-assistant-white-0529314p.html
made-for-esphome: false
difficulty: 4
---

Globe Electric 4-outlet + 2-USB-port smart Wi-Fi power bar with surge protection, sold in North America through Canadian
Tire (SKU #052-9314-6). Controlled out of the box by the Tuya-based "Globe Suite" app; the Wi-Fi module is a Tuya **CB3S**
(Beken BK7231N).

## Basic Configuration

```yaml file=config.yaml
```

## Pinout

Pin map recovered from the stock firmware's provisioned GPIO configuration (`user_param_key`: `rl1_pin`=P8,
`rl2_pin`=P6, `rl3_pin`=P24, `rl4_pin`=P26, `netled1_pin`=P9 active-low, `total_bt_pin`=P7 active-low) and verified in
use:

| GPIO | Function | Notes |
| ---- | -------- | ----- |
| P8 | Relay — Outlet 1 (nearest to plug) | Active high |
| P6 | Relay — Outlet 2 | Active high |
| P24 | Relay — Outlet 3 | Active high |
| P26 | Relay — Outlet 4 | Active high |
| P9 | Status LED (Wi-Fi) | Active low |
| P7 | Button (toggles all outlets) | Active low, needs pull-up |
| P10 | UART1 RX1 | Connect to USB-UART adapter TX when flashing |
| P11 | UART1 TX1 | Connect to USB-UART adapter RX when flashing |

The two USB ports are always powered and not switchable: the stock firmware exposes no USB channel and no GPIO drives them.

## Flashing

1. Remove the screws and open the enclosure; the CB3S module sits on the main PCB.
2. Solder wires to the CB3S module pads: **P10**, **P11**, **3V3**, **GND**, and optionally **CEN**. The module is 3.3 V
   only — do not connect a 5 V adapter directly.
3. Connect a 3.3 V USB-UART adapter crossed over: adapter TX → P10 (RX1), adapter RX → P11 (TX1). A Flipper Zero in
   UART-bridge mode works.
4. **Back up the stock firmware first**, e.g. with
   [ltchiptool](https://docs.libretiny.eu/docs/flashing/tools/ltchiptool/) (Flash → Read), then write the ESPHome image
   (Flash → Write, UF2/RBL file). Short CEN to GND (or power-cycle the bar) repeatedly until the chip enters download
   mode and flashing starts.
5. After the initial serial flash, updates can be delivered over Wi-Fi/OTA as usual.

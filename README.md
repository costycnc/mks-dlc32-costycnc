# MKS-DLC32 CostyCNC

Custom **MKS-DLC32 / ESP32 firmware builds** prepared by CostyCNC.

This repository is **not only for CostyCNC machines**. The MKS-DLC32 is a low-cost ESP32-based CNC controller that can be used as a starting point for many DIY CNC projects, including:

- hot-wire foam cutters
- laser machines
- router/spindle CNC machines
- other experimental CNC machines

The repository provides ready-to-flash `.bin` firmware builds together with a customized WebUI.

**No separate WebUI installation is required after flashing the provided firmware.**

---

## MKS-DLC32 as a DIY CNC controller

The MKS-DLC32 provides the basic functions needed to control a DIY CNC machine:

- stepper motor outputs
- PWM output
- Wi-Fi connectivity
- browser-based WebUI
- GRBL-based control

The exact machine depends on the hardware connected to the controller and the GRBL settings used.

This repository contains both **general MKS-DLC32 resources** and **CostyCNC-specific tested configurations**.

---

## Ready-to-use CostyCNC firmware

| Firmware | Motor setup | $100 | $101 | $102 | CostyCNC |
|---|---|---:|---:|---:|---|
| `1000-28byj.bin` | 28BYJ-48 + A4988, 1/16 | 1024 | 1024 | 1024 | Hobby / Mini |
| `1000-nema.bin` | NEMA17 | 100 | 100 | 100 | Media / XBig |

The values above are the tested motion settings used by these firmware builds.

They are **not universal values** for every motor or mechanical configuration. If you use the firmware on another machine, calibrate `$100`, `$101` and `$102` for your own mechanics.

---

## 28BYJ-48 configuration

The `1000-28byj.bin` build is prepared for:

- 28BYJ-48 geared stepper motors
- external A4988 drivers
- 1/16 microstepping
- GT2 timing belt
- 16-tooth GT2 pulley

The tested configuration uses:

```
$100=1024
$101=1024
$102=1024
```

The important point is that **1024 steps/mm is the value for the complete mechanical system**, not for the 28BYJ-48 motor alone.

---

## NEMA17 configuration

The `1000-nema.bin` build is prepared for the NEMA17 configuration used by CostyCNC Media and XBig machines.

```
$100=100
$101=100
$102=100
```

Again, these values depend on the complete motor, driver, microstepping and mechanical transmission.

---

## Embedded WebUI

The customized WebUI is included in the firmware.

The repository also contains WebUI resources used during development, including:

```
index.html.gz
bordo.html
probe.html
text.html
```

These files do **not** need to be uploaded separately when using the supplied `.bin` firmware.

### probe.html — CostyCNC Image to G-code

`probe.html` is a **CostyCNC-developed image-processing and G-code generation tool**.

It can:

1. start from a black-and-white image,
2. extract the contours,
3. search for the nearest points between contours,
4. join contours into a continuous path,
5. generate G-code for the CostyCNC hot-wire workflow.

The nearest-point joining method is used to reduce unnecessary travel between separate contours.

**probe.html is a CostyCNC tool; it is not a required part of the MKS-DLC32 controller and is not needed for every DIY CNC application.**

---

## PWM output

The MKS-DLC32 provides a PWM output.

For CostyCNC hot-wire machines, PWM is used to control the hot wire. For example:

```gcode
M03 S1000
```

sets the PWM output to the configured maximum value.

The PWM output is not limited to hot-wire cutting. Its actual application depends on the hardware connected to the controller.

---

## CostyCNC machines

| Machine | Motor | Firmware |
|---|---|---|
| Hobby | 28BYJ-48 | `1000-28byj.bin` |
| Mini | 28BYJ-48 | `1000-28byj.bin` |
| Media | NEMA17 | `1000-nema.bin` |
| XBig | NEMA17 | `1000-nema.bin` |

These machines are examples of how the MKS-DLC32 can be used. The controller itself is not restricted to CostyCNC machines.

---

## Flashing the firmware

The supplied `.bin` files are compiled application firmware.

The application firmware is written at:

```
0x1000
```

For a browser-based flashing procedure, use:

**[Espressif esptool-js](https://espressif.github.io/esptool-js/)**

For the complete CostyCNC installation procedure:

**[CostyCNC firmware installation](https://www.costycnc.it/firmware)**

---

## Using the MKS-DLC32 for another DIY CNC

You do not need to own a CostyCNC machine to use the ideas and resources in this repository.

For a new DIY CNC project, the MKS-DLC32 can be used as the controller and configured according to the machine's:

- motors
- drivers
- microstepping
- belt, screw or other transmission
- steps/mm
- PWM-controlled hardware

The CostyCNC firmware files are useful as **tested starting points**, but the GRBL parameters should be adapted to the actual machine.

---

## Repository files

Typical important files are:

```
1000-28byj.bin
1000-nema.bin

index.html.gz
bordo.html
probe.html
text.html
README.md
```

### Firmware

The `.bin` files are the final compiled firmware builds.

### WebUI resources

The HTML files are resources used by the customized WebUI.

The final firmware already contains the WebUI required by the controller.

---

## About CostyCNC

CostyCNC is created by **Boboaca Costel**.

The project follows a simple principle:

> **Maximum result with minimum consumption.**

The goal is to combine inexpensive hardware, simple mechanics and custom software to build practical CNC systems without unnecessary complexity.

---

## Author

**Boboaca Costel – CostyCNC**

[CostyCNC](https://www.costycnc.it/)

[CostyCNC on GitHub](https://github.com/costycnc)


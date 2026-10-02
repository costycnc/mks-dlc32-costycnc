# MKS-DLC32 CostyCNC – Ready-to-Use Firmware & Wireless WebUI

Custom firmware builds for the **MKS-DLC32** controller, based on the original MKS-DLC32 firmware and customized for CostyCNC machines and other DIY CNC projects.

The repository contains **ready-to-flash `.bin` firmware files** with the WebUI, HTML pages, parameters and configuration already embedded in the firmware.

No separate HTML installation is required.

---

## What is this project?

The **MKS-DLC32** is an ESP32-based CNC controller that can be used for different types of machines and applications.

I started from the original MKS-DLC32 firmware and modified it to:

* customize the wireless WebUI
* add and modify WebUI pages and parameters
* configure motion parameters for different motor/mechanical setups
* fix and adapt some parameters and behaviors
* build complete firmware binaries ready to flash

The result is provided as `.bin` firmware files.

These firmware builds are **not limited to CostyCNC machines**.

They can also be used as a starting point for other DIY CNC projects based on the MKS-DLC32, including applications such as:

* hot-wire foam cutting
* laser machines
* router/spindle CNC machines
* other projects using the MKS-DLC32 controller

---

# Firmware binaries

Two ready-to-use firmware configurations are provided.

| Firmware         | Motor configuration   | `$100` | `$101` | `$102` | CostyCNC machines |
| ---------------- | --------------------- | -----: | -----: | -----: | ----------------- |
| `1000-28byj.bin` | 28BYJ-48 + A4988 1/16 |   1024 |   1024 |   1024 | Hobby / Mini      |
| `1000-nema.bin`  | NEMA17                |    100 |    100 |    100 | Media / XBig      |

The numbers in the filenames identify the preconfigured motion setup.

### `1000-28byj.bin`

This firmware is configured for the following tested mechanical setup:

* 28BYJ-48 geared stepper motors
* A4988 stepper drivers
* A4988 configured for **1/16 microstepping**
* GT2 timing belt
* GT2 16-tooth pulley
* approximately 1:64 internal gearbox of the 28BYJ-48

The resulting firmware configuration is:

```text
$100=1024
$101=1024
$102=1024
```

These values correspond to the complete mechanical and electrical configuration above.

They should not be considered a universal `$100` value for every possible 28BYJ-48 setup.

---

### `1000-nema.bin`

This firmware is configured for the NEMA17 configuration used by CostyCNC Media and XBig machines.

The default values are:

```text
$100=100
$101=100
$102=100
```

These values are part of this specific firmware configuration.

If the MKS-DLC32 is used with different motors, drivers, microstepping or mechanical transmission, the `$100`, `$101` and `$102` values should be recalibrated for that machine.

---

# 28BYJ-48 + A4988 configuration

One of the configurations used by CostyCNC combines a **28BYJ-48 geared stepper motor** with an external **A4988 driver**.

The A4988 is configured for **1/16 microstepping**.

The motor is used without opening or modifying its internal PCB.

The common red center-tap wire is simply disconnected externally, and the two coil pairs are connected to the A4988 as required.

This allows the geared 28BYJ-48 motor to be driven through a conventional stepper driver.

The complete chain is:

```text
28BYJ-48
    ↓
internal gearbox
    ↓
A4988 – 1/16 microstepping
    ↓
GT2 belt
    ↓
16T pulley
    ↓
linear movement
```

For the tested CostyCNC configuration this results in:

```text
$100=$101=$102=1024
```

The important point is that the value comes from the **complete system**, not from the 28BYJ-48 motor alone.

---

# Embedded Wireless WebUI

The MKS-DLC32 provides a wireless WebUI through the ESP32.

The firmware builds in this repository contain the customized WebUI resources directly inside the final `.bin`.

The repository also contains the HTML resources used during development/building, such as:

```text
index.html.gz
bordo.html
probe.html
text.html
```

These files are **not separate files that the user has to install after flashing the firmware**.

They are resources used to build the firmware.

The final `.bin` already contains the required WebUI pages.

The basic workflow is therefore:

```text
Flash .bin
   ↓
MKS-DLC32 starts
   ↓
Connect to the controller
   ↓
Open the wireless WebUI
   ↓
Configure / control the CNC
```

---

# PWM output and `M03 S1000`

The MKS-DLC32 provides PWM control that can be used for different applications.

On CostyCNC hot-wire machines, the PWM output is used to control the hot wire.

For example:

```gcode
M03 S1000
```

sets the PWM output to the configured maximum value.

The command itself is **not specific to hot-wire cutting**.

Depending on the connected hardware, PWM can also be used for applications such as:

* laser power control
* spindle-type control
* other PWM-controlled devices

The actual use of the PWM output depends on the hardware connected to the controller.

---

# CostyCNC machine configurations

The firmware configurations correspond to the following CostyCNC machines:

| Machine | Motor    | Firmware         |
| ------- | -------- | ---------------- |
| Hobby   | 28BYJ-48 | `1000-28byj.bin` |
| Mini    | 28BYJ-48 | `1000-28byj.bin` |
| Media   | NEMA17   | `1000-nema.bin`  |
| XBig    | NEMA17   | `1000-nema.bin`  |

The firmware is not restricted to these machines.

The same MKS-DLC32 controller can be adapted to different mechanical configurations by changing the GRBL parameters.

---

# General-purpose MKS-DLC32 controller

The purpose of this project is not to create a controller that only works with one specific CNC.

The MKS-DLC32 is a low-cost ESP32 CNC controller with:

* stepper motor outputs
* PWM output
* wireless connectivity
* WebUI
* GRBL-based control
* support for different CNC applications

This makes it useful as a general controller for DIY machines.

The CostyCNC firmware builds simply provide tested configurations that are already prepared for specific motor and mechanical setups.

---

# Installation

The firmware can be flashed directly to the MKS-DLC32.

The complete installation procedure is documented here:

**[https://www.costycnc.it/firmware](https://www.costycnc.it/firmware)**

The application firmware is written at:

```text
0x1000
```

The `.bin` files in this repository are already compiled and ready to flash.

---

# Repository contents

The repository contains both the final firmware binaries and the WebUI resources used during development.

Typical files include:

```text
1000-28byj.bin
1000-nema.bin

index.html.gz
bordo.html
probe.html
text.html
```

### `.bin` files

These are the **final compiled firmware builds**.

They contain the firmware and the WebUI resources required by the controller.

### HTML files

These are the WebUI source/resources used during the development and compilation process.

They are useful for understanding or modifying the WebUI, but they do not need to be separately uploaded when using the provided `.bin` files.

---

# Why two firmware builds?

The main difference between the two builds is the motion configuration.

### 28BYJ-48 configuration

```text
1000-28byj.bin

$100=1024
$101=1024
$102=1024
```

Designed for the tested:

```text
28BYJ-48
A4988
1/16 microstepping
GT2 belt
16T pulley
```

### NEMA17 configuration

```text
1000-nema.bin

$100=100
$101=100
$102=100
```

Designed for the NEMA17 configurations used by CostyCNC.

---

# Use with other DIY CNC projects

These firmware builds can also be useful if you are building your own CNC based on the MKS-DLC32.

You can start with one of the provided firmware binaries and then adapt the GRBL parameters to your own machine.

For example, you may need to change:

```text
$100
$101
$102
```

depending on:

* motor type
* driver configuration
* microstepping
* pulley size
* belt pitch
* leadscrew pitch
* mechanical transmission

Always calibrate the final steps/mm values for your own machine.

---

# About CostyCNC

CostyCNC is a project created by **Boboaca Costel**.

The approach behind these projects is simple:

> Maximum result with minimum consumption.

The goal is not to make the system unnecessarily complicated.

Instead, the objective is to combine inexpensive hardware, simple mechanics and custom software to obtain a practical CNC system that is easy to understand and use.

---

# Author

**Boboaca Costel – CostyCNC**

[https://www.costycnc.it/](https://www.costycnc.it/)

GitHub:

[https://github.com/costycnc](https://github.com/costycnc)


*Developed by Costel Boboaca (costycnc) – Elevating standard 32-bit hardware into independent wireless CNC automation hubs.*



# mks-dlc32-costycnc

https://github.com/costycnc/esp32-micropython-with-mks-dlc32-board-costycnc

![1](https://github.com/user-attachments/assets/fe5df514-1983-4939-b390-785559e15af2)

Download repository in your computer! Scarica repo in tuo computer!


![5](https://github.com/user-attachments/assets/c151ba32-7359-4c6b-9bfc-29be935a47b7)

![6](https://github.com/user-attachments/assets/aadb6076-3eb1-4fde-addd-6835ca23decf)

Upload files to your makerbase board! Carica i file sulla tua scheda Makerbase!


Alternatively ... in alternativa:

Upload firmware .bin to mks dlc32 board with https://espressif.github.io/esptool-js/

Caricare il firmware .bin sulla scheda MKS DLC32 usando https://espressif.github.io/esptool-js/

<img width="1045" height="580" alt="image" src="https://github.com/user-attachments/assets/2ffe3806-3093-4541-b5ee-62bfae0e59a3" />

<img width="1188" height="712" alt="image" src="https://github.com/user-attachments/assets/d85141fc-2360-46be-8eb9-4dc271a82c64" />








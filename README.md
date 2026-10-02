# MKS-DLC32 CostyCNC

Custom **MKS-DLC32 / ESP32 firmware builds and browser tools** developed and tested by CostyCNC.

This repository is **not only for CostyCNC machines**. The MKS-DLC32 is a low-cost ESP32-based CNC controller that can be used for many DIY machines, including:

- hot-wire foam cutters
- laser machines
- router/spindle CNC machines
- experimental CNC machines
- custom motor-control projects

The repository contains ready-to-flash firmware builds and several browser-based tools used with the MKS-DLC32.

**No separate WebUI installation is required when using the supplied firmware.**

---

## What is the MKS-DLC32?

The MKS-DLC32 is an ESP32-based CNC controller providing:

- stepper motor outputs
- PWM output
- Wi-Fi
- browser-based control
- GRBL-based motion control

The controller can therefore be used as a general DIY CNC control board. The final application depends on the motors, drivers, mechanics and other hardware connected to it.

This repository contains both general MKS-DLC32 resources and **CostyCNC-specific tested configurations and browser tools**.

---

# CostyCNC firmware builds

Two main firmware builds are provided for the CostyCNC machines:

| Firmware | Motor setup | $100 | $101 | $102 | Used on |
|---|---|---:|---:|---:|---|
| `1000-28byj.bin` | 28BYJ-48 + A4988, 1/16 | 1024 | 1024 | 1024 | Hobby / Mini |
| `1000-nema.bin` | NEMA17 | 100 | 100 | 100 | Media / XBig |

These are **tested CostyCNC configurations**, not universal values.

If the firmware is used on another machine, the GRBL steps/mm values must be calibrated for that machine.

---

## 28BYJ-48 configuration

`1000-28byj.bin` is prepared for:

- 28BYJ-48 geared stepper motors
- external A4988 drivers
- 1/16 microstepping
- GT2 timing belt
- 16-tooth GT2 pulley

The tested GRBL values are:

```
$100=1024
$101=1024
$102=1024
```

The **1024 steps/mm value belongs to the complete mechanical system**, including the motor, driver, microstepping, belt and pulley. It is not a specification of the 28BYJ-48 motor alone.

---

## NEMA17 configuration

`1000-nema.bin` is prepared for the NEMA17 configuration used by CostyCNC Media and XBig machines.

```
$100=100
$101=100
$102=100
```

These values also depend on the complete mechanical transmission.

---

# Browser tools included in the repository

The repository contains several HTML tools developed by CostyCNC.

They are not simply decorative WebUI pages. They are small browser applications for preparing images and text, generating G-code, testing the machine and working with the MKS-DLC32.

The main files are:

| File | Purpose |
|---|---|
| `index.html.gz` | Main embedded WebUI |
| `probe.html` | Image/text → G-code tool for the CostyCNC workflow |
| `bordo.html` | Image → black/white silhouette with adjustable border |
| `text.html` | Text/graphics canvas tool with font upload, drawing and PNG export |

---

# probe.html — Image / Text to G-code

`probe.html` is the main CostyCNC browser tool for turning an image or text into machine-ready G-code.

It combines image processing, scaling, G-code generation and direct MKS-DLC32 operations in one page.

### Input

It can work with:

- BMP
- JPG
- PNG
- SVG
- pasted images
- drag-and-drop images
- text entered directly in the page

Text can be generated with the page itself, with controls for text size and contour/name options.

### Image processing

The page uses **Potrace** to convert the image into paths.

It can also work in **contour-only** mode.

For separate contours, CostyCNC processing searches for nearby points and joins contours where appropriate. This is intended to reduce unnecessary travel and create a more practical cutting path.

### Scaling and dimensions

The generated design can be adjusted using:

- X/Y ratios
- desired X/Y dimensions in centimetres
- optional Y locking to X
- DPI selection

The page calculates and displays the resulting dimensions.

### G-code controls

The page provides controls for:

- feedrate `F`
- hot-wire PWM `S`
- G-code editing
- G-code preview
- saving G-code locally

The generated path is also displayed as an SVG preview.

### Machine functions

The page can communicate directly with the MKS-DLC32 and provides functions such as:

- **SQUARE** — generate a simple square test program
- **EXECUTE costycnc.nc** — execute the stored G-code file
- **Scrive on SDCARD** — write G-code to the controller SD card
- **VIEW costycnc.nc** — inspect the stored file
- **STOP MACCHINA** — send the GRBL feed-hold/stop command
- **Save gcode** — download the current G-code

### Rotate table

The page also contains a **ROTATE TABLE** section for rotary-table work.

It provides parameters for:

- rotation amount
- step/rotation
- test rotation
- enabling the rotate-table workflow

The generated G-code can therefore be repeated with Z-axis rotation commands for this type of CostyCNC setup.

### Typical workflow

```
IMAGE / TEXT
     ↓
IMAGE PROCESSING
     ↓
CONTOURS / PATHS
     ↓
SCALING
     ↓
G-CODE
     ↓
PREVIEW
     ↓
SD CARD / EXECUTE
     ↓
MKS-DLC32
     ↓
MACHINE
```

This makes `probe.html` more than an image converter: it is a browser-based preparation and machine-control tool for the CostyCNC workflow.

---

# bordo.html — Silhouette and border tool

`bordo.html` is a small browser image-processing tool.

It allows an uploaded image to be transformed into a black/white silhouette and surrounded by an adjustable border.

Controls include:

- image upload
- border thickness
- black/white threshold
- X position
- Y position
- white or black silhouette

The result is displayed immediately on a canvas.

This page is useful for preparing simple silhouettes and bordered shapes before using them in a CNC image-to-G-code workflow.

---

# text.html — Text and drawing canvas

`text.html` is a browser-based canvas for creating text and simple graphics.

It provides:

- text input
- horizontal and vertical positioning
- font size control
- black and white contour controls
- adjustable internal/external stroke
- custom font upload
- OTF / TTF / WOFF / WOFF2 font support
- freehand brush drawing
- brush size and colour selection
- Undo / Redo
- PNG export

The resulting PNG can then be used as an image source for further processing.

This separates a common task — **creating a simple text image** — from the G-code generation stage.

---

# index.html.gz — Embedded WebUI

`index.html.gz` is the compressed WebUI resource used by the firmware.

The supplied firmware already contains the WebUI required by the controller, so these HTML files do not normally need to be uploaded separately.

The individual HTML pages in this repository are also useful as readable/development versions of the browser tools.

---

# PWM output

The MKS-DLC32 provides a PWM output.

For CostyCNC hot-wire machines, PWM is used to control the hot wire. For example:

```gcode
M03 S1000
```

sets the PWM value to the configured maximum.

The PWM output is not restricted to hot-wire machines. Its use depends on the hardware connected to the controller.

---

# CostyCNC machines

| Machine | Motor | Firmware |
|---|---|---|
| Hobby | 28BYJ-48 | `1000-28byj.bin` |
| Mini | 28BYJ-48 | `1000-28byj.bin` |
| Media | NEMA17 | `1000-nema.bin` |
| XBig | NEMA17 | `1000-nema.bin` |

These machines are examples of how the MKS-DLC32 is used in the CostyCNC system. The controller itself is not restricted to these machines.

---

# Flashing the firmware

The supplied `.bin` files are compiled ESP32 application firmware.

The application firmware is written at:

```
0x1000
```

For browser-based flashing, use:

**Espressif esptool-js**

https://espressif.github.io/esptool-js/

CostyCNC installation instructions:

https://www.costycnc.it/firmware

---

# Using the MKS-DLC32 for another DIY CNC

You do not need to own a CostyCNC machine to use this repository.

The MKS-DLC32 can be adapted to another DIY CNC by configuring the controller for the actual:

- motors
- drivers
- microstepping
- mechanical transmission
- steps/mm
- PWM-controlled hardware

The CostyCNC firmware builds are best considered **tested starting points**. They should not be assumed to be correct for every machine.

---

# Repository files

Important files include:

```
1000-28byj.bin
1000-nema.bin

index.html.gz
probe.html
bordo.html
text.html
README.md
```

### Firmware

The `.bin` files are compiled firmware builds ready to flash.

### Browser tools

The HTML files contain browser-based tools for image preparation, text creation, G-code generation and MKS-DLC32 interaction.

---

# About CostyCNC

CostyCNC is created by **Boboaca Costel**.

The project follows a simple principle:

> **Maximum result with minimum consumption.**

The goal is to combine inexpensive hardware, simple mechanics and custom software to build practical CNC systems without unnecessary complexity.

---

# Author

**Boboaca Costel — CostyCNC**

https://www.costycnc.it/

https://github.com/costycnc

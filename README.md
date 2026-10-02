# MKS-DLC32 CostyCNC – Firmware & Wireless WebUI for Hot-Wire Foam Cutting

Custom compiled firmware binaries (`.bin`) and embedded WebUI components for the **Makerbase MKS-DLC32 (ESP32)** controller, adapted by **CostyCNC** for low-cost CNC hot-wire foam cutting.

The project turns the MKS-DLC32 into a simple wireless controller for CostyCNC hot-wire machines, allowing the machine to be controlled directly from a web browser without requiring a desktop CNC application.

---

## 🔬 Technical Architecture & Embedded Deployment

This repository provides ready-to-use firmware binaries and embedded web interface components, avoiding the need for users to build the firmware locally with PlatformIO or the Arduino IDE.

The MKS-DLC32 runs the motion-control firmware while its ESP32 provides the Wi-Fi connection and hosts the CostyCNC browser interface.

The basic architecture is:

```text
Computer / Smartphone
        │
        │ Wi-Fi
        ▼
┌──────────────────────┐
│   MKS-DLC32 / ESP32  │
│                      │
│   GRBL Motion Control│
│   CostyCNC WebUI     │
└──────────┬───────────┘
           │
           ▼
     Stepper Drivers
           │
           ▼
      CNC Mechanics
           │
           ▼
    Hot-Wire Foam Cutter
```

---

## 🔌 Real-Time Motion Profile Binaries

### `1000-nema.bin`

Production firmware configuration for CostyCNC machines using conventional bipolar stepper motors such as **NEMA 17 or NEMA 23**.

The actual steps/mm calibration depends on the mechanical transmission, pulley, belt, screw, motor and microstepping configuration of the machine.

### `1000-28byj.bin`

Specialized firmware configuration for CostyCNC machines using inexpensive **28BYJ-48 geared stepper motors** with **A4988 bipolar drivers**.

This configuration allows the 28BYJ-48 to be used without opening or permanently modifying its internal PCB.

---

## ⚙️ 28BYJ-48 + A4988 Configuration

The CostyCNC configuration uses:

* 28BYJ-48 geared stepper motor
* internal motor gearbox
* A4988 bipolar stepper driver
* **1/16 microstepping**
* GT2 timing belt
* **16-tooth GT2 pulley**

The 28BYJ-48 is normally supplied as a 5-wire unipolar motor.

For the CostyCNC configuration, the common center-tap wire is disconnected externally and the two coil pairs are connected to the four outputs of the A4988.

### Wiring modification

The motor does **not** need to be opened.

The CostyCNC wiring procedure is:

1. Disconnect the **red center-tap wire**.
2. Identify the two coil pairs.
3. Reposition the required wires in the connector according to the CostyCNC wiring sequence.
4. Connect the four coil wires to the A4988 motor outputs.
5. Configure the A4988 for **1/16 microstepping**.

> **Important:** wire colours can vary between different 28BYJ-48 manufacturers and versions. Always verify the coil pairs before connecting the motor.

### Video tutorial

[Unipolar 28BYJ-48 on MKS-DLC32 CostyCNC](https://www.youtube.com/shorts/9iet-oZP_rM)

---

## 📐 Why `$100 = 1024`?

The value `$100 = 1024` is not simply a property of the 28BYJ-48 motor.

It is the result of the complete CostyCNC electrical and mechanical configuration:

* 28BYJ-48 geared motor
* internal reduction gearbox
* A4988 driver
* **1/16 microstepping**
* GT2 belt with **2 mm pitch**
* **16-tooth pulley**

The 16-tooth GT2 pulley produces:

```text
16 × 2 mm = 32 mm
```

of linear movement per pulley revolution.

Taking into account the motor's internal gearbox and the **1/16 microstepping configuration of the A4988**, the CostyCNC setup is calibrated to:

```text
$100=1024
$101=1024
$102=1024
```

For the tested CostyCNC mechanical configuration:

```text
X100 → 100 mm
Y100 → 100 mm
Z100 → 100 mm
```

The value should nevertheless be verified on the individual machine because 28BYJ-48 motors and gearboxes can vary between manufacturers.

### CostyCNC 28BYJ-48 calibration

| Component                  | Configuration      |
| -------------------------- | ------------------ |
| Motor                      | 28BYJ-48           |
| Internal gearbox           | approximately 1:64 |
| Driver                     | A4988              |
| Microstepping              | 1/16               |
| Belt                       | GT2                |
| Belt pitch                 | 2 mm               |
| Pulley                     | 16 teeth           |
| Pulley movement/revolution | 32 mm              |
| GRBL calibration           | **1024 steps/mm**  |

---

## 🔬 Why No Internal PCB Modification Is Required

Many 28BYJ-48 bipolar conversion tutorials modify the motor's internal PCB.

The CostyCNC configuration does not require this.

The center-tap is disconnected externally, leaving the two coil pairs available for the bipolar A4988 driver.

The important requirement is correct identification of the two coil pairs and correct connection to the A4988 outputs.

This makes it possible to use the inexpensive geared 28BYJ-48 motor without cutting traces or modifying the motor's internal PCB.

---

## 🌐 Embedded Wireless WebUI

The CostyCNC WebUI is designed to run directly from the ESP32 inside the MKS-DLC32.

The main interface is provided as:

```text
index.html.gz
```

The compressed HTML/JavaScript payload reduces the amount of flash storage required by the embedded interface.

Additional CostyCNC web components include:

* `bordo.html`
* `probe.html`
* `text.html`

These provide additional browser-based functions for machine control, calibration and text-related operations.

The user connects to the MKS-DLC32 through Wi-Fi and opens the interface from a normal web browser.

No dedicated desktop CNC application is required for the basic browser-based workflow.

---

## 🧩 Repository Files

| File             | Description                                               |
| ---------------- | --------------------------------------------------------- |
| `1000-nema.bin`  | Firmware configuration for NEMA stepper installations     |
| `1000-28byj.bin` | Firmware configuration for 28BYJ-48 + A4988 installations |
| `index.html.gz`  | Compressed embedded CostyCNC WebUI                        |
| `bordo.html`     | Additional WebUI functions                                |
| `probe.html`     | Calibration/probing functions                             |
| `text.html`      | Text-related WebUI functions                              |
| `read_4mb.b`     | Flash/read utility                                        |
| `MKSLaserTool`   | Supporting MKS-DLC32 utility                              |

---

## 🛠️ Installation

### 1. Connect the MKS-DLC32

Connect the MKS-DLC32 to the computer using USB.

### 2. Select the correct firmware

Choose the firmware according to the motor configuration:

```text
1000-nema.bin
```

or:

```text
1000-28byj.bin
```

### 3. Flash the ESP32

Use a compatible ESP32 browser-based flashing tool such as [Espressif Web Tools](https://espressif.github.io/esptool-js/).

Flash the firmware according to the memory layout required by the specific binary.

> **Important:** do not assume that every `.bin` file should always be written at `0x0`. A complete ESP32 flash image and a firmware-only binary can require different flash addresses. Follow the instructions corresponding to the supplied image.

### 4. Connect through Wi-Fi

After flashing, power-cycle the MKS-DLC32 and connect to its Wi-Fi network.

Open the CostyCNC WebUI from a browser.

---

## ⚠️ Calibration and Hardware Safety

Before operating the machine:

* verify the motor wiring
* verify the A4988 microstepping configuration
* verify motor current limiting
* test each axis at low speed
* verify the actual travel distance
* verify the direction of each axis
* make sure the machine can move freely

Do not assume that the calibration values from one mechanical configuration are correct for another machine.

---

## 🎯 Why Use an MKS-DLC32 for Hot-Wire Cutting?

A hot-wire foam cutter has different requirements from a milling machine.

There is no cutting tool applying significant mechanical force to the material and no spindle is required.

CostyCNC therefore focuses on a simple architecture:

```text
Simple mechanics
       +
Low-cost stepper motors
       +
MKS-DLC32 / ESP32
       +
Wi-Fi WebUI
       +
Hot wire
```

The result is a compact controller that can be operated directly from a browser.

---

## 🔗 Related CostyCNC Projects

The MKS-DLC32 is part of a larger CostyCNC software ecosystem.

Related projects include:

* [MKS-DLC32 Custom WebUI and Web Commands](https://github.com/costycnc)
* [CostyCNC Firmware](https://github.com/costycnc)
* [ESP32 MicroPython with MKS-DLC32](https://github.com/costycnc)

See the [CostyCNC GitHub repositories](https://github.com/costycnc) for additional projects and experiments.

---

## 👤 Author

**Costel Boboaca – CostyCNC**

CostyCNC develops low-cost CNC hot-wire foam-cutting machines and software tools for controlling them.

Website: [costycnc.it](https://www.costycnc.it/)

GitHub: [github.com/costycnc](https://github.com/costycnc)

---

## 💡 CostyCNC Philosophy

> **Maximum result with minimum consumption.**

The objective is not to make the system more complicated.

The objective is to use simple mechanics, inexpensive electronics and browser-based software to obtain a practical CNC hot-wire foam-cutting machine.


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








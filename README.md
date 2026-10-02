# MKS-DLC32 CostyCNC – Embedded Firmware Ecosystem & Wireless WebUI

An optimized hardware deployment repository featuring custom compiled production binaries (`.bin`) and embedded file-system components for the **Makerbase MKS-DLC32 (ESP32)** controller board, tailored specifically for high-efficiency hot wire foam cutters.

---

## 🔬 Technical Architecture & Embedded Deployment

This repository bypasses complex local toolchain compilation (PlatformIO/Arduino IDE) by provisioning plug-and-play binary structures and custom client-side web assets designed to host local user interfaces directly from the MCU's internal flash memory.

### 🔌 Real-Time Motion Profile Binaries
* **`1000-nema.bin`:** Production firmware pre-configured with acceleration and step-generation metrics optimized for heavy bipolar industrial stepper motors (NEMA 17/23 series), ensuring high-torque continuous vector tracking.
* **`1000-28byj.bin`:** Specialized low-cost deployment binary utilizing altered step/direction mapping and timing scaling matrices to drive unipolar 5V geared stepper motors (28BYJ-48), democratizing hardware access for DIY builds.

### 🌐 Embedded Web Server Optimization (`index.html.gz`)
* **Gzip Compression Footprint:** The primary control dashboard is pre-compressed into a Gzip binary block (`index.html.gz`). This reduces raw HTML/JavaScript asset payloads to minimal memory blocks, enabling instantaneous delivery across the local network from the internal ESP32 HTTP stateless web server.
* **Edge UI Additions:** Includes dedicated localized boundary modules (`bordo.html`), dynamic material calibration layers (`probe.html`), and custom text geometry interpreters (`text.html`) that load dynamically inside the browser shell.

---

## 🛠️ Step-by-Step Installation Guide / Guida all'Installazione

### 1. Wireless Web Interface Flash (Metodo via Browser)
You do not need to install local desktop software. The firmware binaries can be synthesized directly to your Makerbase board via standard web serial protocols.
1. Connect the MKS-DLC32 board to your computer via USB.
2. Launch the Web-Serial browser installer utility: [Espressif Web Tool (esptool-js)](https://github.io)
3. Select your target `.bin` firmware structure matching your physical stepper architecture (NEMA or 28BYJ).
4. Flash the binary block to the MCU flash address range (`0x0`).

### 2. Uploading WebUI Assets to ESP32 Flash
Once the base firmware is running, upload the asset files (`index.html.gz`, `bordo.html`, etc.) via the controller's integrated Web Upload interface or micro-SD routing to activate the standalone wireless dashboard interface.

---

## 📋 Cross-Repository References
* **MicroPython Alternative Logic:** For developers looking to run Python scripts directly on this hardware array, see our secondary core: [esp32-micropython-with-mks-dlc32-board-costycnc](https://github.com)

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








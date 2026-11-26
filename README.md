# Orange Pi 4 Pro — MainsailOS / Klipper Image (CAN-Enabled)
Pre-built, shrunk disk image for the **Orange Pi 4 Pro** featuring **Klipper**, **Moonraker**, and **Mainsail**, with **CAN bus support enabled** and popular Klipper add-ons pre-installed.

## Image Details
* **File Name:** `OPi4Pro_Mainsail_CAN_ENABLED.img.xz`
* **Target Board:** Orange Pi 4 Pro
* **Minimum Storage Required:** 32 GB+ microSD card or eMMC module
* **Compressed Size:** ~3.5 GB (expands to ~14.5 GB before filesystem resize)
* **Default SSH / System Login:** `orangepi` / `orangepi`
* **SHA-256 Checksum:**
  ```text
  e74f8747bf2a917a965b419924a08cbb7b043afed7352fd4918b5f01d77f9f8a
  ```

---

## Pre-Installed Software & Features
* **Web Interface:** Mainsail + Moonraker
* **Display:** KlipperScreen
* **Camera Streaming:** Crowsnest
* **Remote Monitoring / Mobile:** Obico & Mobileraker Companion
* **Tuning & Macros:** Klipper TMC Autotune & KAMP (Klipper Adaptive Meshing & Purging)
* **Hardware / Bus Support:** Kernel CAN bus support enabled (`can0` interface available; may need to be brought up / configured for your specific CAN adapter bitrate)

---

## Flashing Instructions

1. **Verify Your Download (Optional):**
   * **Windows (PowerShell):**
     ```powershell
     Get-FileHash .\OPi4Pro_Mainsail_CAN_ENABLED.img.xz -Algorithm SHA256
     ```
   * **Linux / macOS:**
     ```bash
     sha256sum -c OPi4Pro_Mainsail_CAN_ENABLED.img.xz.sha256
     ```
   * Confirm the output matches: `e74f8747bf2a917a965b419924a08cbb7b043afed7352fd4918b5f01d77f9f8a`

2. **Flash the Image:**
   * Open **BalenaEtcher** or **Raspberry Pi Imager**.
   * Select **`OPi4Pro_Mainsail_CAN_ENABLED.img.xz`** directly (do **not** extract it first—both tools decompress `.xz` automatically while flashing).
   * Select your **32 GB or larger** microSD card or eMMC module and click **Flash**.

3. **First Boot:**
   * Insert the flashed microSD card or eMMC module into your Orange Pi 4 Pro and power it on (allow 1–2 minutes for initial boot).
   * Connect via Ethernet (or configure Wi-Fi via `sudo nmtui` / `sudo orangepi-config`) and find the board's IP address from your router or KlipperScreen.
   * Open `http://<orange-pi-ip>` in your browser to access **Mainsail**.

---

## Post-Boot Setup Checklist

1. **Expand the Root Filesystem:**
   * Because this image is shrunk to ~14.5 GB, expand the partition to use your full 32 GB+ card/eMMC capacity if it does not auto-expand on first boot:
     ```bash
     sudo orangepi-config
     ```
     Go to **System** -> **Resize**, then reboot (`sudo reboot`).

2. **Configure Your MCU (USB or CAN):**
   * **USB MCU:** Connect your printer board, run `ls /dev/serial/by-id/*` over SSH, and update `[mcu] serial:` in `printer.cfg`.
   * **CAN Bus (`can0`):** CAN support is compiled and enabled in the kernel. Bring up `can0` / configure `/etc/network/interfaces.d/can0` (or `systemd-networkd`) with your desired bitrate (e.g., `1000000`) and add your toolhead/board `canbus_uuid:` to `printer.cfg`.

3. **Link Your Accounts & Change Default Password:**
   * Link your **Obico** and **Mobileraker** accounts if you plan to use remote monitoring/notifications.
   * Change the default `orangepi` password (`passwd`) before exposing the device on your network.

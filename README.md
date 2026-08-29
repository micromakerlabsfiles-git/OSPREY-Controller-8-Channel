# Osprey Controller — Operator & Installer Guide

Welcome to the **Osprey Controller (8-Channel Smart Audio Sequencer & Power Manager)**! This guide is written for operators, sound engineers, and installers. It explains how to operate the device using the tactile rotary encoder dial, high-contrast OLED display, and Web Controller dashboard.

---

## 1. Operating the Device

The Osprey Controller features a **tactile rotary encoder dial** with an integrated push switch, a dedicated **Emergency / Action button (PBS-1)**, and a high-contrast OLED screen.

### Rotary Dial & Button Controls:
- **Rotate Knob (Clockwise / Counter-Clockwise)**:
  - Scrolls smoothly through active menu modes with animated carousel transitions on the OLED display.
  - Modes activate automatically after the configured **Dial Turn Activation Delay** (e.g. 1.0s settle time) or immediately when clicked. This scroll-settle feature prevents relay chattering during fast knob rotation.
- **Short Press Dial**:
  - **While Scrolling**: Immediately settles and activates the highlighted mode without waiting for the timer.
  - **In Manual Channel Modes (MANUAL CH 1–8, MANUAL ALL ON)**: Toggles the highlighted relay channel **ON** / **OFF**.
  - **In Power Groups (Modes 25–34)**: Activates or deactivates the power group with individual soft-start and shutdown delays.
  - **In Auto Modes**: Cancels the active auto timer and returns to Standby OFF.
- **Long Press Knob**:
  - Hold the rotary knob down (0.5s to 5.0s, configurable) to toggle between **STANDBY (Safe FIFO All-OFF)** and **MANUAL ALL ON**.
- **Emergency Button (PBS-1)**:
  - Single press triggers the configured emergency action immediately with **zero delay** (instant hardware override).
  - Displays a 3-second countdown popup with active channel names and stays in the selected mode when dismissed.

---

## 2. Smart Audio Controller by OSPREY (Power Sequencer & Anti-Pop System)

When high-power audio equipment (TVs, Inverters, Mixers, Crossover Boards, DSPs, and MOSFET Amplifiers) powers on simultaneously, high inrush currents produce loud speaker pops and amplifier thumps.

The Osprey Controller eliminates this using **per-channel entry (turn-ON) and exit (turn-OFF) delays**:

| Equipment | Channel | Startup (Entry Delay) | Shutdown (Exit Delay) | Function |
|---|---|---|---|---|
| **TV / Display** | CH 1 | **0.0s** (Instant) | **3.0s** (Delayed) | Powers on first, stays alive until audio stops |
| **Main Inverter** | CH 2 | **0.0s** (Instant) | **3.0s** (Delayed) | Power source on first, cuts last |
| **Audio Mixer** | CH 3 | **0.5s** (Soft-start) | **2.0s** | Stabilizes audio signal |
| **Crossover Board** | CH 4 | **0.5s** (Soft-start) | **2.0s** | Clean frequency split |
| **Audio DSP** | CH 5 | **3.0s** (Delayed ON) | **0.0s** (Immediate OFF) | **Eliminates startup pop**; cuts first on power down |
| **Subwoofer Amp** | CH 6 | **4.0s** (Delayed ON) | **0.5s** | Powers on after DSP is stable |
| **Main Mid/Horn Amp**| CH 7 | **4.0s** (Delayed ON) | **0.5s** | Powers on after DSP is stable |
| **Spare / Accent** | CH 8 | **0.0s** | **0.0s** | Auxiliary equipment |

---

## 3. OLED Visual Indicators

- **Top Status Badges [1] to [8]**:
  - **Solid White Box + Black Digit**: Channel is currently **ACTIVE (ON)**.
  - **Outlined Box + Center Dots**: Channel is **PENDING (Entry Delay counting down)**.
  - **Outlined Box + White Digit**: Channel is **INACTIVE (OFF)**.
- **Screensaver**: Inactivity screensaver appears when in Standby OFF and no delays or relays are active. Rotate dial or click to wake instantly. Screensaver is automatically suppressed whenever any relay channel is energized.

---

## 4. Connecting Equipment to the Relay Channels

The Osprey Controller has **8 independent relay channels** (CH 1–8) for switching AC or DC electrical loads.

### Wiring Instructions:
1. **Safety First**: Turn off all main power sources before touching wiring.
2. Connect the power feed (Live / 12V / 24V Positive) to the **COM (Common)** screw terminal.
3. Connect the load lead going to your amplifier/DSP/inverter to the **NO (Normally Open)** screw terminal. Neutral / Ground wires bypass the controller directly.
4. When the channel energizes, the relay closes and powers the connected equipment.

---

## 5. Web Controller Manager & Flasher (`index.html`)

Open the Web Manager in Google Chrome, Microsoft Edge, or Opera on Desktop:

### 1. In-Browser Firmware Flashing (No Arduino / VS Code Needed)
* Connect the ESP32 board via USB.
* Click **⚡ Flash / Install Firmware Now** on the connect screen.
* The browser will automatically flash the latest 4-part firmware package (`bootloader`, `partitions`, `boot_app0`, `firmware`) directly over Web Serial.

### 2. Device Configuration & Live Sync
* Click **Connect & Configure Controller** to read live configurations from the board.
* **Branding & Channel Labels**: Rename Relay Channels 1–8 with descriptive names (e.g. TV, DSP, SUB AMP, VOCALS) and change the startup boot screen text.
* **Live Relay Test**: Click **⚡ Test Click** next to any channel to immediately test physical relay hardware.
* **Layout & Font Settings**: Choose between *Vertical Carousel* or *Horizontal Flipper* menu layouts, and select your preferred OLED screen font (Helvetica, Monospace, Profont).
* **Display Controller Selection**: Choose between `SH1106 (1.3" OLED)` and `SSD1306 (0.96" OLED)`.
* **Buzzer Audio Feedback**: Check or uncheck the buzzer toggle to enable/silence dial feedback tones.
* **Timers & Settle Delays**: Adjust knob long-press thresholds, dial turn activation delays, auto mode duration, and screensaver timeouts.
* **Power Sequencer & Groups**: Set individual turn-ON (entry) and turn-OFF (exit) delays per channel, and configure multi-channel preset groups.
* **Emergency Actions**: Select the exact mode or relay triggered instantly when the emergency button is pressed.
* **Backup & Restore**:
  - Click **📥 Export Config (.txt)** to backup and save your settings into a readable `.txt` profile.
  - Click **📤 Upload Config (.txt)** to restore or share saved profiles across multiple controllers.
* **Save & Sync**: Click **Save & Sync to Controller** to flash and persist all settings in microcontroller NVS flash memory.

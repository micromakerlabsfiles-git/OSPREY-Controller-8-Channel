# Osprey Controller — Operator Guide

Welcome to the **Osprey Controller (8-Channel Audio Sequencer & Power Manager)**! This guide is written for operators and installers. It explains how to operate the device using the rotary encoder dial, high-contrast OLED display, and Web Controller dashboard.

---

## 1. Operating the Device

The Osprey Controller features a **tactile rotary encoder dial** with an integrated push switch, a dedicated **Emergency / Action button (PBS-1)**, and a high-contrast OLED screen.

### Rotary Dial & Button Controls:
- **Rotate Knob (Clockwise / Counter-Clockwise)**: Smoothly scroll through active menu modes with animated visual transitions.
- **Short Press Dial**:
  - In **Manual Channel Modes** (MANUAL CH 1–8, MANUAL ALL ON): Toggles the highlighted relay channel **ON** / **OFF**.
  - In **Power Groups** (Modes 25–29): Activates or deactivates the power group with individual soft-start/shutdown delays.
  - In **Auto Modes**: Cancels the active auto timer and returns to Standby OFF.
- **Long Press Dial (Hold for 1 sec)**:
  - When in **STANDBY OFF**: Switches directly to **MANUAL ALL ON**.
  - When in **ANY OTHER MODE**: Returns immediately to **STANDBY OFF** (applying exit delays for safe FIFO shutdown).
- **Emergency Button (PBS-1)**:
  - Single press triggers the configured emergency action immediately with **zero delay** (instant GPIO override).
  - Displays a 3-second countdown popup with active channel names and stays in the selected mode when dismissed.

---

## 2. Kerala Bus Audio Power Sequencer (Anti-Pop Delay System)

When high-power audio equipment (TV, Inverters, Mixers, Crossover Boards, DSPs, and MOSFET Amplifiers) powers on together, high-voltage inrush currents produce loud speaker pops and amplifier thumps.

The Osprey Controller eliminates this using **per-channel entry and exit delays**:

| Equipment | Channel | Startup (Entry Delay) | Shutdown (Exit Delay) | Function |
|---|---|---|---|---|
| **TV / Display** | CH 1 | **0 ms** (Instant) | **3000 ms** (Delayed) | Powers on first, stays alive until audio stops |
| **Main Inverter** | CH 2 | **0 ms** (Instant) | **3000 ms** (Delayed) | Power source on first, cuts last |
| **Audio Mixer** | CH 3 | **500 ms** (Soft-start) | **2000 ms** | Stabilizes audio signal |
| **Crossover Board** | CH 4 | **500 ms** (Soft-start) | **2000 ms** | Clean frequency split |
| **Audio DSP** | CH 5 | **3000 ms** (Delayed ON) | **0 ms** (Immediate OFF) | **Eliminates startup pop**; cuts first on power down |
| **Subwoofer Amp** | CH 6 | **4000 ms** (Delayed ON) | **500 ms** | Powers on after DSP is stable |
| **Main Mid/Horn Amp**| CH 7 | **4000 ms** (Delayed ON) | **500 ms** | Powers on after DSP is stable |
| **Spare / Accent** | CH 8 | **0 ms** | **0 ms** | Auxiliary equipment |

---

## 3. OLED Visual Indicators

- **Top Status Badges [1] to [8]**:
  - **Solid White Box + Black Digit**: Channel is currently **ACTIVE (ON)**.
  - **Outlined Box + Center Dots**: Channel is **PENDING (Entry Delay counting down)**.
  - **Outlined Box + White Digit**: Channel is **INACTIVE (OFF)**.
- **Screensaver**: Inactivity screensaver appears when in Standby OFF and no delays or relays are active. Rotate dial or click to wake instantly.

---

## 4. Connecting Equipment to the Relay Channels

The Osprey Controller has **8 independent relay channels** (CH 1–8).

### Wiring Instructions:
1. Turn off all power sources before touching wiring.
2. Connect the power feed (Live / 12V / 24V Positive) to the **COM (Common)** screw terminal.
3. Connect the load lead going to your amplifier/DSP/inverter to the **NO (Normally Open)** screw terminal.
4. When the channel energizes, the relay closes and powers the connected equipment.

---

## 5. Web Controller Manager (`index.html`)

Open the Web Manager in Google Chrome, Microsoft Edge, or Opera on Desktop:
1. Connect the ESP32 controller via USB.
2. Click **Connect Controller** to read configurations.
3. Customize channel names, entry/exit delays, power groups, fonts, and screensaver timeouts.
4. Click **⚡ Load Kerala Bus DSP Preset** to auto-fill recommended audio delays.
5. Click **Save & Sync** to store settings in microcontroller NVS flash.

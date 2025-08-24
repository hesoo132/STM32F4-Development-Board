## 📘 STM32F4Core Board Schematic Review (PDF)

> Document: [`STM32F4Core.pdf`](STM32F4-Demo-Board.pdf) · **6 pages** · KiCad-based STM32F4 core-board schematic  
> Goal: Summarize **module functions** and list **bring-up checkpoints** for quick hardware validation.

---

### 🔎 Overview
This note summarizes the **STM32F4Core** schematic built around **STM32F407VETx**.  
The board regulates **5 V → 3.3 V**, exposes rich **GPIO ports**, supports **USB communication**, provides accurate clocks (**8 MHz / 32.768 kHz**), and offers **JTAG/SWD** for firmware debugging.

---

### 🧭 Summary

| Module | Key Function | Main Parts/Connectors | Notes |
|---|---|---|---|
| **Power** | 5 V to 3.3 V regulation | **AMS1117**, POW_LED_1, SW_Slide_DPDT | Board power on/off & indication |
| **Pin Ports** | Break-out of MCU GPIOs | J3 / J4 / J5 | PA–PE groups, OSC/RESET lines |
| **USB** | USB data & power switching | **USB Mini-B (J2)**, **MIC2025-1YM** | D+/D−, protection/matching parts |
| **CRYSTAL** | System/RTC clocks | 8 MHz, 32.768 kHz crystals | Reset switch (SW2), **20-pin JTAG (J12)** |
| **MCU** | System control | **STM32F407VETx** | VCAP/bypass caps, **boot jumper (J11)** |

---

### 🧩 Module Details

#### 1) Power
- **Function**: Convert external **5 V** to **3.3 V** with **AMS1117** to feed the MCU and peripherals.  
- **Composition**: AMS1117, power LED (**POW_LED_1**), **slide switch (SW_Slide_DPDT)** for power control.  
- **Note**: LED shows power status; switch enables on/off management.

#### 2) Pin Ports
- **Function**: Export **STM32F407VETx** GPIOs to external headers (**J3/J4/J5**).  
- **Composition**: **PA, PB, PC, PD, PE** groups, external oscillator pins (**OSC_IN/OUT**, **OSC32_IN/OUT**), **RESET**.  
- **Note**: Useful for sensors, actuators, or other MCUs.

#### 3) USB
- **Function**: USB communication via **USB Mini-B (J2)** with **MIC2025-1YM** power switch management.  
- **Composition**: D+/D− data lines, power switch, protection/matching resistors and capacitors.  
- **Note**: Current limiting and protection considered for safe PC connection/power sourcing.

#### 4) CRYSTAL (Clocks & Debug)
- **Function**: Accurate clocks: **8 MHz** (system) and **32.768 kHz** (RTC).  
- **Composition**: Crystals + load capacitors, **reset switch (SW2)**, **20-pin JTAG (J12)**.  
- **Note**: Enables robust **JTAG/SWD** firmware debug.

#### 5) MCU
- **Model**: **STM32F407VETx** (ARM Cortex-M4).  
- **Function**: Central control and peripheral interface.  
- **Composition**: **VCAP**/bypass capacitors, **boot-mode jumper (J11)**, power and ground pins.

---

### 🧪 Quick Bring-up Checklist
- **Power**: 5 V in → stable 3.3 V rail, power LED on.  
- **Clocks**: 8 MHz / 32.768 kHz crystals soldered/spec-correct → MCU clock configuration verified.  
- **Reset/Boot**: **SW2** works; **J11** boot jumper (Flash/Bootloader) set as intended.  
- **USB**: Device enumerates over D+/D−; **MIC2025-1YM** power-switch behavior OK.  
- **GPIO Ports**: Verify J3/J4/J5 pinout → simple toggle/input tests.  
- **Debug**: **JTAG/SWD (J12)** connects; flash & debug session stable.

---

### 🖼️ (Optional) Page/Block Captures
Recommended path:

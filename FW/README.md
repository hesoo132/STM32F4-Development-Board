# 🔧 STM32F4 Development Board Firmware

## 🔎 Overview
This firmware is designed for the **STM32F407ZET6 Development Board**.  
It provides examples for GPIO, UART, LCD, Timer, PWM, ADC, DAC, I2C and peripheral control.  
The code demonstrates **interrupt-based handling, polling-based logic, and peripheral drivers** using STM32 HAL.

---

## 📊 System Block Diagram
![System Diagram](../docs/images/system-diagram.png)

---

## ⚙️ Features

### 1) LED Control
- **B1** → Interrupt-based toggle of LD1  
- **B2** → Polling: Turn ON LEDs  
- **B3** → Polling: Turn OFF LEDs  
- **B4** → Interrupt: Sync LD2 with LD1 (using flag `led_turned_on`)  

👉 Implemented via:
- `HAL_GPIO_EXTI_Callback()` → Handle external interrupts for B1/B4  
- `while(1)` loop polling with `HAL_GPIO_ReadPin()` for B2/B3  

---

### 2) UART – LED Control
- UART1 interrupt receive enabled (`HAL_UART_Receive_IT`)  
- Case 1: Characters `A–C` turn ON LD1, `D–F` turn OFF LD1  
- Case 2: Strings `"LED ON"` and `"LED OFF"` toggle LD2  

👉 Implemented via:
- `HAL_UART_RxCpltCallback()` with buffer and string comparison (`strncmp`)  

---

### 3) UART – LCD Display
- Input characters displayed on **20×4 LCD** via UART interrupt  
- `"Clear"` command clears LCD and resets cursor row  
- Auto line feed with row increment  
- Uses `lcd_Init(4,20), lcd_setCursor(), lcd_string()`  

---

### 4) Timer Control (Reaction Test)
- LD1 (PG13) starts ON, LD2 (PG14) OFF  
- LD1 turns OFF randomly after 1–5 sec (using `rand()`)  
- Then LD2 turns ON randomly after 1–5 sec  
- LCD shows **“R U Ready…?”** and then reaction speed time  
- Button PA0 → Stop timer and freeze reaction time  

👉 Implemented via:
- `HAL_TIM_PeriodElapsedCallback(TIM7)` for random timing  
- `HAL_GPIO_EXTI_Callback()` for stop/resume with button  

---

### 5) Timer Control 2 – Dual LED Toggle
- TIM7 → Toggle LD1 every 1 sec  
- TIM6 → Toggle LD2 every 2 sec  

👉 Implemented via:
- Two base timers with independent callbacks in `HAL_TIM_PeriodElapsedCallback()`  

---

### 6) PWM Control
- **Button 1** → PWM 100Hz  
- **Button 2** → PWM 400Hz  
- **Button 3** → Duty cycle 25%  
- **Button 4** → Duty cycle 75%  

👉 Implemented via:
- TIM2 PWM configuration, updating `PSC`, `ARR`, `CCR1`  
- Flags set by EXTI interrupts (B1–B4)  

---

### 7) PWM Applications
- **SG90 Servo Motor (TIM4_CH2)**  
- **Buzzer (TIM3_CH1)** → 500Hz–1kHz tone  
- **DC Motor (TIM2_CH1/CH2)** → Forward/reverse rotation with duty change  

👉 Real-time PWM frequency adjustment via `PSC` or `ARR` update.  

---

### 8) DAC
- **DAC1 (PA4)** outputs analog voltage (0–3.3V, 12-bit)  
- Example: Constant 1.6V, triangle wave, sine wave with TIM7 interrupt  

👉 Implemented via:
- `HAL_DAC_Start()`  
- `HAL_DAC_SetValue()`  

---

### 9) I2C – EEPROM + LCD
- I2C1 used to read/write external EEPROM (0xA0)  
- Write 10 bytes → Read back & display on LCD  
- Must use `I2C_MEMADD_SIZE_16BIT` for addressing  

👉 Implemented via:
- `HAL_I2C_Mem_Write()` and `HAL_I2C_Mem_Read()`  
- Display results with `lcd_setCurStr()`  

---

## ✅ Build & Flash
- **IDE**: STM32CubeIDE  
- **Toolchain**: ARM-GCC with HAL drivers  
- **Programmer**: ST-LINK V2/V3  
- **Example Flash Command**:
```bash
st-flash write build/firmware.bin 0x8000000

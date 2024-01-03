# STMConfig
## STM 32 Hardware Design and Custom Configuration using KiCad 7
The STM32 microcontroller model used here is the Blue Pill (STM32F103C8T6). It hosts the ARM Cortex M3 Microprocessor with a 32-bit RISC core operating at a 72 MHz frequency, and high-speed embedded memories. In this project, It has been interfaced with 
 + 1 USB (Micro-USB 2.0)
 + 1 UART line
 + 1 I2C2 line
 + Custom Oscillator
 + Regulator
   
#### Pin allocation and Power Configuration
There is a simple configuration performed therefore, although connected, the VBAT, and VDDA (Analog) are not actively used. To prevent power fluctuations and stabilize transitions, each Voltage source is provided with a decoupling capacitor and a bulk decoupling capacitor for all of them collectively. For the analog source, we need to provide an analog source linked to the collective VDD Supply. Since the analog voltage source is more sensitive, it is connected to a `ferrite bead ` of 120Ohm, 100MHz.

The `Reset` pin is connected to a pull-up circuit with a capacitor to prevent spurious resets. The `Boot0` pin used to enable or disable the upload and debugging of a program onto a board via the STMLINK is connected to a switch with a relatively high resistance.

##### Custom Oscillator
The `PD0` and `PD1` Pins are the oscillator i/o pins. For our custom oscillator, we have chosen the `SMD3225-4P` HSE(High-Speed External) Crystal Oscillator. Despite the fact that the MCU has an in-built oscillator, an external oscillator not only provides better control and more precise clock speed but also enables the provision of a greater clock frequency. We have utilized it in order to provide greater frequency of `16MHz` using the oscillator.
In accordance with the documentation, we connect two load capacitors whose value is determined by

```
CLoad = (Coscillator - CStray) * 2
CLoad = (10-5) *2 (stray capacitance ranges between 3 and 5 Farads)
CLoad = 10pF
```




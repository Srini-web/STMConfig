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
The `PD0` and `PD1` Pins are the oscillator i/o pins. For our custom oscillator, we have chosen the `SMD3225-4P` HSE(High-Speed External) Crystal Oscillator. Although the MCU has an in-built oscillator, an external oscillator not only provides better control and more precise clock speed but also enables the provision of a greater clock frequency. We have utilized it to provide greater frequency of `16MHz` using the oscillator.
Per the documentation, we connect two load capacitors whose value is determined by :-

```
CLoad = (Coscillator - CStray) * 2
CLoad = (10-5) *2 (stray capacitance ranges between 3 and 5 Farads)
CLoad = 10pF
```

##### USB
A Micro-USB (B) has been connected to the `PA11` and `PA12` pins which act as serial wire input and output pins. Specification is `STM: AN4879` . This circuitry is attached to a pull-up resistor circuit. 

##### Regulator
A regulator is required to filter the Input voltage provided through the `VBUS` input. This also serves as a power supply indicator using an LED in parallel. The specification is a linear resistor `AMS 3.3V` variant.

##### UART and I2C2
The UART and I2C2 pins are assigned to the `PB6`, `PB7`, and the `PB10`, `PB11` pins. The Pin assignment has been done using the STM32CubeIDE. After the assignment, the files are exported into KiCad. It is available in the `STM32F103C8T6_pinout.ioc` file. The clock circuitry generated also gives insights into the oscillator connection. A no connect `X` is attached to unused pins to avid ERC Violations.

<img width="850" alt="stmcube" src="https://github.com/Srini-web/STMConfig/assets/77874288/b5b9f136-7822-489b-870f-3aed455355f7">

<img width="830" alt="stmcubeclkckt" src="https://github.com/Srini-web/STMConfig/assets/77874288/1c261f14-0ca7-4ed1-9351-b00939b40636">

### Schematic

<img width="618" alt="schematic" src="https://github.com/Srini-web/STMConfig/assets/77874288/49c8a594-d6ce-4f34-988c-a66eac990eaa">

During ERC, A power source conflict is created at the analog voltage source, Due to 2 conflicting voltage sources which can be avoided using a power flag.

### Footprint Assignment

Footprint assignment is done to ensure each component specification is done for PCB design and Manufacturing

<img width="882" alt="footprint" src="https://github.com/Srini-web/STMConfig/assets/77874288/1cebd564-403b-4e14-90d9-d09ccee1fcc7">

### Planning (Layout)
It is a 2 layer board  into which the schematic footprints are imported and components are placed guided by the rat's nest links. Edge planning and mount holes are also added. Component labeling and notes are created using silkscreen. This is where the mount holes are also added, edge cuts are made defining board edges and the 3D model of the USB is appended to the footprint using the `629105150521 (rev1).stp` CAD file downloaded from the component site.

<img width="515" alt="placement" src="https://github.com/Srini-web/STMConfig/assets/77874288/0e70436d-6fb6-47d6-aa31-bb3cdb82af07">

### Routing 
+ Path routing is essential and dependent on the efficiency of Planning. Two tracks have been used. Namely the 0.3 mm and 0.5 mm tracks. The smaller tracks are mostly used for signal routing and the larger/ broader track is used for power routing and ground vias.
+ Certain connects are connected by a power puddle during power routing. If this is not done properly, This may cause a copper imbalance leading to Tombstoning.

##### Signal Routing
<img width="501" alt="SignalRouting" src="https://github.com/Srini-web/STMConfig/assets/77874288/c8bc0ad4-54a2-4d2b-994a-76cb3abd87cb">

##### Power Routing
<img width="805" alt="PowerRouting" src="https://github.com/Srini-web/STMConfig/assets/77874288/1bc99696-644f-4ada-a9ea-fb44e9bbaa7c">

##### 3D view after P&R

<img width="345" alt="3DModelF" src="https://github.com/Srini-web/STMConfig/assets/77874288/1362558e-90f8-4d52-bd56-51f6d61473f6">

<img width="376" alt="3DModelB" src="https://github.com/Srini-web/STMConfig/assets/77874288/133a748a-ef4c-4ded-9f74-21c554a21ef6">

### Manufacturing
Following DRC clearance, to prepare the manufacturing model, the material list is generated from the schematic. After specifying and generating the drill hole list, The Gerber files are generated. All of these are available in the `Manufacturing` folder.

<img width="856" alt="Gerber1" src="https://github.com/Srini-web/STMConfig/assets/77874288/dbe55105-9e72-4ffb-8cab-369d89082f75">

<img width="850" alt="Gerber2" src="https://github.com/Srini-web/STMConfig/assets/77874288/1a1d027c-2a65-4573-9d3d-b37595162e38">



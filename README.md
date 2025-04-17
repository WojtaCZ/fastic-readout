# FastIC+ readout 

This repository contains all the resources for the FastIC+ readout developed at CERN. **Please note that this project is a work-in-progress as of now and thus this repository does not yet contain the resources needed to manufacture a fully working device.**

The ultimate goal of this project was to develop a system utilizing the FastIC+ chips that could be used at CERN to quickly assess measurements, by companies to simplify prototyping with the FastIC+ chips or by schools to use them in experiments.
The requirements were for it to be:
- versatile to allow for both simple and demanding tasks,
- easy to use for both novices and experts,
- standalone all-in-one device requiring no external scientific instruments,
- miniature and portable to be taken anywhere,
- accessible for schools and companies.

<div align="center">
  <img src="https://github.com/WojtaCZ/fastic-readout-mech/blob/main/images/preview.png?raw=true" width="100%" />
</div>

## The FastIC+
The FastIC+ is an **8-channel** front-end for photo detectors such as SiPM, PMT or MCP, capable of precisely measuring the **Time-of-Arrival** (ToA) and energy of photons hitting the detector from the **Time-over-Treshold** (ToT).

The detection part of the chip implements current mode front-ends capable of working with both positive and negative polarity sensors with dynamic range of **5 µA to 20 mA** and capacitance range of **10 pF to 1 nF**. The eight channels can either operate **independently** or in a **summation mode** and can be highly configured to suffice demanding user needs. Several triggering schemes are available for discriminating the photon impacts by their energy as well as external trigger input and trigger output to allow synchronization of multiple chips. Three different measurement modes,
- ToA (Non linear ToT),
- Energy (from ToT),
- Hybrid (ToA + Energy),
are present to allow the user to optimize for measurement of ToA, ToT or both. The maximum detection rate of the impacts is approximately **2 MHz in the linear mode** and **50 MHz in non-linear mode**. A 40 MHz low jitter reference clock is required for the chip to operate.

The measured ToA and ToT are outputted separately for each channel as a digital pulse, where the beginning of the pulse marks the ToA of the photon and the length of the pulse encodes the energy (ToT). These channels are exposed to the user and can be used for photon detection. However, in order to enhance the time measurement of the ToA and ToT pulses and offload this processing from the user, a **25 ps time bin TDC** for digitizing the pulses directly on the chip has been implemented and the **Aurora 64B/66B** used to transfer the digitized data at speeds from 80 Mb/s to 1.28 Gb/s.

## The readout
A readout system comprising **two** FastIC+ chips, thus **16 channels** has been developed. The readout is capable of ~ **1.6 MHz hit rate** per FastIC+ (200 kHz per channel) and integrates all the necessary electronics to perform the FastIC+ communication and configuration. **High voltage (75 V max) supply** for biasing the sensors is implemented as well. **Trigger IN/OUT** of the FastIC+ is exposed on SMA connectors to allow for daisy chaining multiple boards. The **USB C 2.0 interface** is used for transferring the data to the host PC and powering the device. The system is based on STM32 allowing for easy firmware customization by the user if required. Manufacturing files for the readout are available in the [fastic-readout-hw](https://github.com/WojtaCZ/fastic-readout-hw.git) repository. The firmware files in the [fastic-readout-fw](https://github.com/WojtaCZ/fastic-readout-fw.git) repository. 

<div align="center">
  <img src="https://github.com/WojtaCZ/fastic-readout-hw/blob/V2.0/outputs/images/3d-top.png" width="45%" />
  <img width="8%"> </img>
  <img src="https://github.com/WojtaCZ/fastic-readout-hw/blob/V2.0/outputs/images/3d-bottom.png" width="45%" /> 
</div>

### Communication 
The readout implements two interfaces for communication with the device. 
#### CDC interface
The first one is a CDC interface, that emulates a COM port over USB. This interface implements a simple human-readable protocol that allows the user to control all the necessary functions of the device. The following commands are available, where square braces indicate a choice from one of the options, curly braces indicate an expected value and triagular braces indicate an optional flag.

`get readout status`\
Returns the status of the readout system.\
`get readout uid`\
Returns the unique id of the readout system.\
`get readout firmware`\
Returns the information about the firmware of the readout.\
`get hv enable`\
Returns the state of the high-voltage power supply.\
`set hv enable [true/false]`\
Enables/disables the high-voltage power supply.\
`get hv current`\
Returns the current drawn from the high-voltage power supply.\
`get hv voltage`\
Returns the voltage setpoint of the high-voltage power supply.\
`get hv pid`\
Returns the high-voltage regulator P, I, D values.\
`set hv pid {float: P} {float: I} {float: D}`\
Sets the high-voltage regulator P, I, D values.\
`set hv voltage {float: voltage}`\
Sets the high-voltage power supply voltage setpoint. A float number is expected.\
`get fastic register [1/2] {hex byte: address}`\
Returns the value of the specified FastIC+ register\
`set fastic register [1/2] {hex byte: address} {hex byte: value} <f>`\
Sets the value of the specified FastIC+ register. The optional **f** suffix is required in some cases where register writes could affect the device functionality. The readout will inform the user about the need to use this suffix.\
`get fastic voltage [1/2]`\
Returns the VMON voltage of the selected FastIC+.\
`get fastic syncreset [1/2]`\
Returns the value of the selected FastIC+ SRST pin.\
`set fastic syncreset [1/2] [high/low]`\
Sets the FastIC+ SRST pin to the provided state\
`get fastic calpulse [1/2]`\
Returns the state of the injection pulse generator for the selected FastIC+.\
`set fastic calpulse [1/2] [enable/disable]`\
Enables/disables the calibration pulse generation for the selected FastIC+.\
`get fastic time [1/2]`\
Returns the state of the selected FastIC+ time pin.\
`get fastic aurora [1/2]`\
Returns the state of the Aurora stream for the selected FastIC+.\
`set fastic aurora [1/2] [enable/disable]`\
Enables/disables the Aurora stream for the selected FastIC+.\
`get userboard status`\
Returns the status of the userboard.\
`get userboard uid`\
Returns the ID of the userboard.\
`get userboard name`\
Returns the name for the userboard.\
`set userboard name {string: name}`\
Sets the userboard name to the specified vstring value. The maximum number of characters is 64.\
`get userboard writeprotect`\
Returns the state of the userboard write protect bit.\
`set userboard writeprotect [true/false]`\
Enables/disables the userboard write protect.\
`get userboard init`\
Checks if the userboard memory has been initialized.\
`set userboard init `\
Initializes the userboard memory when a non-initialized userboard is first connected to a readout.\
`get userboard voltage `\
Returns the high-voltage power supply voltage preset stored in the userboard.\
`set userboard voltage {float: voltage}`\
Sets the high-voltage power supply voltage preset stored in the userboard. A float number is expected.\
`get userboard register [1/2] {hex byte: address}`\
Returns the specified register preset of the selected FastIC+ from the userboard.\
`set userboard register [1/2] {hex byte: address} {hex byte: value}`\
Sets the specified register preset of the selected FastIC+ from the userboard.\
`set userboard tomemory`\
Copies the current FastIC+ configuration and high-voltage setpoint into the userboard memory.\
`set userboard frommemory`\
Loads the FastIC+ configuration and high-voltage setpoint from the userboard memory and applies it.\

#### Vendor interface
A second way to communicate with the device is via a binary vendor control transfer. This will be documented more in the future.

## The user board
A simple user board has been developed as well to be used standalone or as a template for a custom board. The template provided includes **16 generic SiPM footprints** arranged in a 4x4 grid. Each SiPM channel is biased from the readout HV supply and separately filtered to minimize the chance of cross triggering of the sensors. An EEPROM is implemented to allow for storage of configuration on the user board itself. The manufacturing files are available in the [fastic-userboard-hw](https://github.com/WojtaCZ/fastic-userboard-hw.git) repository.

<div align="center">
  <img src="https://github.com/WojtaCZ/fastic-userboard-hw/blob/main/outputs/images/3d-top.png" width="39%" />
  <img width="20%"> </img>
  <img src="https://github.com/WojtaCZ/fastic-userboard-hw/blob/main/outputs/images/3d-bottom.png" width="39%" /> 
</div>


## Mechanical
Mechanical files for the readout system enclosure are made available in the [fastic-readout-mech](https://github.com/WojtaCZ/fastic-readout-mech.git) repository. These files are created such that they can be manufactured on a regular 3D printer.

<div align="center">
  <img src="https://github.com/WojtaCZ/fastic-readout-mech/blob/main/images/top.png" width="45%" />
  <img width="8%"> </img>
  <img src="https://github.com/WojtaCZ/fastic-readout-mech/blob/main/images/bottom.png" width="45%" /> 
</div>

<div align="center">
  <img src="https://github.com/WojtaCZ/fastic-readout-mech/blob/main/images/rear.png" width="45%" />
  <img width="8%"> </img>
  <img src="https://github.com/WojtaCZ/fastic-readout-mech/blob/main/images/front.png" width="45%" /> 
</div>

## Documentation
The in-depth documentation of the device concept and electronics, as well as user's manual for operating the device are present in the [fastic-readout-doc](https://github.com/WojtaCZ/fastic-readout-doc.git).

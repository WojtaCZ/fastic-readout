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
A readout system comprising **two** FastIC+ chips, thus **16 channels** has been developed. The readout is capable of **~ 1.6 MHz hit rate per FastIC+** (200 kHz per channel) and integrates all the necessary electronics to perform the FastIC+ communication and configuration. **High voltage (75 V max) supply** for biasing the sensors is implemented as well. **Trigger IN/OUT** of the FastIC+ is exposed on SMA connectors to allow for daisy chaining multiple boards. The **USB C 2.0 interface** is used for transferring the data to the host PC and powering the device. The system is based on STM32 allowing for easy firmware customization by the user if required. Manufacturing files for the readout are available in the [fastic-readout-hw](https://github.com/WojtaCZ/fastic-readout-hw.git) repository. The firmware files in the [fastic-readout-fw](https://github.com/WojtaCZ/fastic-readout-fw.git) repository. 

<div align="center">
  <img src="https://github.com/WojtaCZ/fastic-readout-hw/blob/V2.0/outputs/images/3d-top.png" width="45%" />
  <img width="8%"> </img>
  <img src="https://github.com/WojtaCZ/fastic-readout-hw/blob/V2.0/outputs/images/3d-bottom.png" width="45%" /> 
</div>

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
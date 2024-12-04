# FastIC+ readout 

This repository contains all the resources for the FastIC+ readout developed at CERN. **Please note that this project is unfinished as of now and thus this repository does not yet contain the resources needed to manufacture a fully working device.**

The ultimate goal of this project was to develop a system utilizing the FastIC+ chips that could be used at CERN to quickly assess measurements, by companies to simplify prototyping with the FastIC+ chips or by schools to use them in experiments.
The requirements were for it to be:
- versatile to allow for both simple and demanding tasks,
- easy to use for both novices and experts,
- standalone all-in-one device requiring no external scientific instruments,
- miniature and portable to be taken anywhere,
- accessible for schools and companies.

Based on these, the full electronics, firmware and mechanical parts were developed and made available in this repository.

<div align="center">
  <img src="https://github.com/WojtaCZ/fastic-readout-mech/blob/main/images/preview.png?raw=true" width="100%" />
</div>

## Electronics and firmware
The source files for the electronics of the readout itself and a easily modifiable user board can be found in the [fastic-readout-hw](git@github.com:WojtaCZ/fastic-readout-hw.git) and [fastic-userboard-hw](git@github.com:WojtaCZ/fastic-userboard-hw.git) repositories. A C/C++ firmware for the readout board is provided in the [fastic-readout-fw](git@github.com:WojtaCZ/fastic-readout-fw.git) repository.

## Mechanical
Mechanical files for the readout system enclosure are made available in the [fastic-readout-mech](git@github.com:WojtaCZ/fastic-readout-mech.git) repository. These files are created such that they can be manufactured on a regular 3D printer.

## Documentation
The in-depth documentation of the device concept, as well as user's manual for operating the device are present in the [fastic-readout-doc](git@github.com:WojtaCZ/fastic-readout-doc.git). 
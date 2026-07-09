# Nanoprocessor-Extended Folder Guide

This README is a quick index for the hardware project folder.

## Folders

- Bitstream: Generated bitstream output.
- Building Blocks: Reusable low-level VHDL components (adders, muxes, registers, decoders).
- Constraint File: BASYS3 XDC constraints.
- Docs: Operation manual and hardware port mapping files.
- Package Files: Shared type and constant packages.
- Source Files: Main processor modules grouped by function.
- Testbench Files: Simulation testbenches for top-level and submodules.

## Most Important Files

- Source Files/Top File/NanoProcessor.vhd
- Source Files/Instruction Decoder/Instruction_Decoder.vhd
- Source Files/Program ROM/Program_ROM.vhd
- Constraint File/Nanoprocessor_Const.xdc
- Docs/Extended_NanoProcessor_Manual.txt
- Docs/Port Mapping.jpg

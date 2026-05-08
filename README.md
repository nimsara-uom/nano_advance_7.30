# NanoProcessor Extended (Basys3 / VHDL)

This repository contains an FPGA-oriented 4-bit educational nanoprocessor implemented in VHDL, including core datapath modules, testbenches, constraints, and a sample ROM program.

## Repository Layout

- `/home/runner/work/nano_advance_7.30/nano_advance_7.30/Nanoprocessor-Extended/Source Files`  
  Top-level processor and major functional blocks (ALU, ROM, decoder, register bank, etc.)
- `/home/runner/work/nano_advance_7.30/nano_advance_7.30/Nanoprocessor-Extended/Building Blocks`  
  Reusable low-level components (registers, adders, muxes, decoders)
- `/home/runner/work/nano_advance_7.30/nano_advance_7.30/Nanoprocessor-Extended/Package Files`  
  Shared type and constant definitions (`BusDefinitions.vhd`, `constants.vhd`)
- `/home/runner/work/nano_advance_7.30/nano_advance_7.30/Nanoprocessor-Extended/Testbench Files`  
  Testbenches for subsystem and top-level simulation
- `/home/runner/work/nano_advance_7.30/nano_advance_7.30/Nanoprocessor-Extended/Constraint File/Nanoprocessor_Const.xdc`  
  Basys3 pin assignments
- `/home/runner/work/nano_advance_7.30/nano_advance_7.30/Nanoprocessor-Extended/Bitstream`  
  Generated bitstream artifacts (if present)

## Top-Level Interface

Defined in:
`/home/runner/work/nano_advance_7.30/nano_advance_7.30/Nanoprocessor-Extended/Source Files/Top File/NanoProcessor.vhd`

### Inputs
- `Clock`
- `Reset` (asynchronous)

### Outputs
- `Data_7[3:0]` (register R7)
- `Data_6[3:0]` (register R6)
- `S_7Seg[6:0]` (7-segment segments)
- `anode[3:0]` (digit enable; fixed to `"1110"` in current design)
- Status flags: `Overflow`, `Zero`, `Equal`, `LessThan`, `GreaterThan`

## Processor Summary

- 8 registers × 4-bit (`R0`–`R7`)
- 3-bit program counter (8 instruction ROM depth)
- Instruction width: 12 bits
- Operations:
  - `MOVI`
  - `ADD`, `SUB`, `NEG`
  - `AND`, `OR`, `XOR`
  - `MUL`
  - `CMP`
  - `JZR`

Opcode and ALU constants are in:
`/home/runner/work/nano_advance_7.30/nano_advance_7.30/Nanoprocessor-Extended/Package Files/constants.vhd`

## Current ROM Program

Defined in:
`/home/runner/work/nano_advance_7.30/nano_advance_7.30/Nanoprocessor-Extended/Source Files/Program ROM/Program_ROM.vhd`

```
0: "101110000101"  -- MOVI R7,5
1: "101100000011"  -- MOVI R6,3
2: "001111100000"  -- ADD R7,R6
3: "001101110001"  -- SUB R6,R7
4: "001111100101"  -- MUL R7,R6
5: "001101110011"  -- OR  R6,R7
6: "001111100100"  -- XOR R7,R6
7: "001111000111"  -- ALU/CMP form (encoding selects RegA=R7, RegB=R4)
```

## Basys3 Hardware Mapping (Active Pins)

From:
`/home/runner/work/nano_advance_7.30/nano_advance_7.30/Nanoprocessor-Extended/Constraint File/Nanoprocessor_Const.xdc`

- Clock: `W5`
- Reset (BTNU): `U18`
- LEDs:
  - `Data_7[0..3]`: `U16 E19 U19 V19`
  - `Data_6[0..3]`: `W18 U15 U14 V14`
  - `Equal`: `U3`
  - `LessThan`: `P3`
  - `GreaterThan`: `N3`
  - `Zero`: `P1`
  - `Overflow`: `L1`
- 7-segment:
  - `S_7Seg[0..6]`: `W7 W6 U8 V8 U5 V5 U7`
  - `anode[0..3]`: `U2 U4 V4 W4`

## Important Behavior Notes

- `Overflow` and `Zero` top-level outputs are gated by `Load_Select` and only actively reflect ALU-write cycles in `NanoProcessor.vhd`.
- `Equal/LessThan/GreaterThan` are driven directly from ALU comparator outputs.
- Comparator uses signed comparisons (`numeric_std.signed`) in:
  `/home/runner/work/nano_advance_7.30/nano_advance_7.30/Nanoprocessor-Extended/Source Files/Comparator/Comparator_4_bit.vhd`
- 7-segment output is a hex LUT for the nibble value (0–F) of `R7`.

## Simulation

Use the testbenches under:
`/home/runner/work/nano_advance_7.30/nano_advance_7.30/Nanoprocessor-Extended/Testbench Files`

Examples include:
- `NanoProcessor_TB.vhd`
- `ALU_TB.vhd`
- `Instruction_Decoder_TB.vhd`
- `Register_Bank_TB.vhd`
- `Multiplier_4_bit_TB.vhd`

Run them in Vivado Simulator (xsim) or your preferred VHDL simulator by compiling package files first, then building blocks/source files, then testbench files.

## Synthesis / Bitstream (Vivado)

1. Create/open a Vivado project for Basys3.
2. Add VHDL files from:
   - `Package Files`
   - `Building Blocks`
   - `Source Files`
3. Set top module to `NanoProcessor`.
4. Add constraint file:
   `/home/runner/work/nano_advance_7.30/nano_advance_7.30/Nanoprocessor-Extended/Constraint File/Nanoprocessor_Const.xdc`
5. Run synthesis, implementation, and bitstream generation.
6. Program the FPGA and verify LEDs/7-segment behavior.

## Included Manual

The original text manual is at:
`/home/runner/work/nano_advance_7.30/nano_advance_7.30/Nanoprocessor-Extended/Extended_NanoProcessor_Manual.txt`

This README is intended to be the implementation-accurate quick reference for contributors.

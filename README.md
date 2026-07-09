# NanoProcessor Extended (4-bit FPGA NanoProcessor)

This repository contains an extended VHDL implementation of a 4-bit nano processor targeted for the BASYS3 FPGA board.

The design includes:
- 8-register register bank (R0-R7), each 4 bits
- 12-bit instruction word with 8-word ROM program memory
- ALU operations: ADD, SUB, NEG, AND, OR, XOR, MUL, CMP
- Conditional jump (JZR)
- Hardware output mapping to LEDs and 7-segment display

## Repository Layout

- [Nanoprocessor-Extended](Nanoprocessor-Extended): Main hardware project
- [Nanoprocessor-Extended/Source Files](Nanoprocessor-Extended/Source%20Files): Core processor modules
- [Nanoprocessor-Extended/Testbench Files](Nanoprocessor-Extended/Testbench%20Files): Simulation testbenches
- [Nanoprocessor-Extended/Constraint File](Nanoprocessor-Extended/Constraint%20File): BASYS3 pin constraints
- [Nanoprocessor-Extended/Docs](Nanoprocessor-Extended/Docs): Manual and board mapping assets
- [Nanoprocessor-Extended/Bitstream](Nanoprocessor-Extended/Bitstream): Generated bitstream output

## Architecture Summary

The top module is [Nanoprocessor-Extended/Source Files/Top File/NanoProcessor.vhd](Nanoprocessor-Extended/Source%20Files/Top%20File/NanoProcessor.vhd).

Data/control flow at a high level:
1. Program Counter provides a 3-bit ROM address.
2. Program ROM outputs one 12-bit instruction.
3. Instruction Decoder generates register select, ALU op, immediate, and jump control.
4. Register bank and multiplexers feed ALU operands.
5. ALU result or immediate is written back via load selector.
6. Register outputs and flags are driven to LEDs and display logic.

## Instruction Set

Instruction width: 12 bits.

Primary opcodes:
- `10`: MOVI (move immediate)
- `00`: ALU operation
- `01`: NEG operation
- `11`: JZR (jump if selected register equals zero)

ALU sub-opcodes used when opcode is `00`:
- `0000`: ADD
- `0001`: SUB
- `0010`: AND
- `0011`: OR
- `0100`: XOR
- `0101`: MUL
- `0111`: CMP

References:
- [Nanoprocessor-Extended/Package Files/constants.vhd](Nanoprocessor-Extended/Package%20Files/constants.vhd)
- [Nanoprocessor-Extended/Source Files/Instruction Decoder/Instruction_Decoder.vhd](Nanoprocessor-Extended/Source%20Files/Instruction%20Decoder/Instruction_Decoder.vhd)

## Board Mapping and I/O

Primary outputs:
- `Data_7[3:0]` on LED3-0
- `Data_6[3:0]` on LED7-4
- Status flags (Overflow, Zero, Equal, LessThan, GreaterThan) on LED15-11
- 7-segment output via `S_7Seg[6:0]` and `anode[3:0]`

Constraint file:
- [Nanoprocessor-Extended/Constraint File/Nanoprocessor_Const.xdc](Nanoprocessor-Extended/Constraint%20File/Nanoprocessor_Const.xdc)

Manual and visual mapping:
- [Nanoprocessor-Extended/Docs/Extended_NanoProcessor_Manual.txt](Nanoprocessor-Extended/Docs/Extended_NanoProcessor_Manual.txt)
- [Nanoprocessor-Extended/Docs/Port Mapping.jpg](Nanoprocessor-Extended/Docs/Port%20Mapping.jpg)

## Simulation and Validation

Main system testbench:
- [Nanoprocessor-Extended/Testbench Files/NanoProcessor_TB.vhd](Nanoprocessor-Extended/Testbench%20Files/NanoProcessor_TB.vhd)

Additional unit-level testbenches are available for ALU, comparator, multiplier, decoder, register bank, and multiplexers in:
- [Nanoprocessor-Extended/Testbench Files](Nanoprocessor-Extended/Testbench%20Files)

## Build and Run (Vivado)

1. Open Vivado and create a new RTL project.
2. Add VHDL files from:
	- `Package Files`
	- `Building Blocks`
	- `Source Files`
3. Add constraints from [Nanoprocessor-Extended/Constraint File/Nanoprocessor_Const.xdc](Nanoprocessor-Extended/Constraint%20File/Nanoprocessor_Const.xdc).
4. Set top module to `NanoProcessor`.
5. Run synthesis, implementation, and bitstream generation.
6. Program BASYS3 and verify LEDs/7-segment behavior.

## Notes

- The processor is 4-bit, so arithmetic overflow is expected outside signed range -8 to +7.
- The included ROM program demonstrates arithmetic, logic, multiplication, and compare behavior.

## Status

Documentation and folder organization were cleaned up by grouping standalone documentation assets under:
- [Nanoprocessor-Extended/Docs](Nanoprocessor-Extended/Docs)
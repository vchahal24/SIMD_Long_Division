# SIMD Long Division Hardware Accelerator
### VHDL, FPGA, RTL Design

## Overview
This repository contains a **SIMD (Single Instruction, Multiple Data) long division hardware accelerator** implemented in **pure VHDL** and designed for FPGA deployment. The system performs **four independent unsigned integer divisions concurrently**, demonstrating hardware parallelism through replicated datapaths rather than sequential execution.

The system integrates arithmetic datapaths, control logic, on-chip memory, and a seven-segment display interface into a complete, synthesizable FPGA design with simulation-based verification.

## Highlights
- 4-way parallel (SIMD) long division
- Pure RTL VHDL (no HLS)
- Utilizes shift–subtract long division algorithm
- Distinct separation of datapath, control, and I/O
- Verified via top-level VHDL testbench
  
### Performance & Parallelism
Compared to a **sequential design** that reuses a **single divider**, this SIMD architecture **reduces total execution time** by up to **75%** when processing **four independent division operations**.

## Architecture Summary
The design is composed of the following subsystems:

### 1. Input Data Storage
Dividend/divisor pairs are stored in an internal ROM. Each SIMD lane receives its own operand pair, enabling independent computation.

### 2. Parallel Long Division Datapaths (x4)
A parameterized `NBitLongDivision` module implements binary long division using:
- Shift and subtract operations
- Comparator-driven control
- Registered intermediate values
This module is instantiated four times at the top level, forming a true SIMD execution engine where all divisions proceed simultaneously.

### 3. Output Registering & Formatting
Results are captured in registers to ensure stable outputs and are formatted into hexadecimal form for display.

### 4. Display Control
A display controller multiplexes output digits and drives a seven-segment decoder, allowing results to be observed directly on FPGA hardware.

## File Structure
`
├── NBitLongDivision.vhd           # Core long division datapath
`

`
├── InputPairsROM.vhd              # Operand ROM
`

`
├── REGOUT.vhd                     # Output registers
`

`
├── DivisionToDisplayInterface.vhd # Result formatting logic
`

`
├── DisplayDecoder.vhd             # Hex to seven-segment decoder
`

`
├── DisplayController.vhd          # Display multiplexing & timing
`

`
├── TopLevel_LongDivision.vhd      # System integration
`

`
├── TopLevel_LongDivision_TB.vhd   # Verification testbench
`

`
├── SIMDiv1.xdc                    # FPGA pin constraints
`

`
└── README.md
`

## FPGA Implementation
- Target: Xilinx FPGA (e.g., Nexys A7–class board)
- Toolchain: Xilinx Vivado
- I/O: Seven-segment display

### Build Flow (Vivado)
1. Create a new RTL project
2. Add all `.vhd` source files
3. Add `SIMDiv1.xdc` as the constraints file
4. Set `TopLevel_LongDivision.vhd` as the top module
5. Run synthesis and implementation
6. Generate the bitstream and program the FPGA

## Design Constraints
- Unsigned operands (up to 6 bits)
- Fixed input pairs for clarity
- Focus on **parallel throughput**, not pipelining
- Division-by-zero is assumed not to occur
- Display timing optimized for human readability
